<template>
  <div id="container">
    <div class="mod-select">
        choose a model :
        <select id="selectBox" v-on:change="chooseGrave" v-model="chooseGraveValue">
          <option v-for="name in nameList" v-bind:value="name">{{name}}</option>
        </select>
        <!-- <button id="stopAnimation" v-on:change="stopRotation" value=""></button> -->
    </div>

  </div>
</template>

<script>
const THREE = require('three');
const ColladaLoader = require('three-collada-loader');
const OrbitControls = require('three-orbitcontrols');

export default {
  name: 'app',
  data () {
    return {
      // msg: 'Welcome to Your Vue.js App'
      camera: null,
      scene: null,
      renderer: null,
      chooseGraveValue: "",
      apiEndPoint: 'http://localhost:8000/',
      nameList: []
    }
  },
  methods: {
    chooseGrave:function() {
      var model = this.chooseGraveValue;
      model = this.apiEndPoint+model;
      this.sceneSetup();
      this.loadModel(model);
      this.animate();
    },

    sceneSetup: function() {
      this.scene = new THREE.Scene();

      var container = document.getElementById('container');
      this.camera = new THREE.PerspectiveCamera(4, window.innerWidth / window.innerHeight, 10, 20000);
      this.camera.position.set(0.01,70,20);

      this.camera.lookAt(this.scene.position);

      this.renderer = new THREE.WebGLRenderer({antialias: true});
      this.renderer.shadowMap.enabled = true;


      this.renderer.setPixelRatio(window.devicePixelRatio);
      this.renderer.setSize(window.innerWidth, window.innerHeight);
      container.appendChild(this.renderer.domElement);

      this.controls = new OrbitControls(this.camera, this.renderer.domElement);
      // this.controls.enableDamping = true
      // this.controls.dampingFactor = 0.25
      // this.controls.enableZoom = false
      this.controls.autoRotate = true;


      // this.controls.addEventListener('change', renderModel);

      var light = new THREE.DirectionalLight(0x505050,1);

      var directionalLight = new THREE.DirectionalLight( 0xffffff, 0.8 );
      var hemiLight = new THREE.HemisphereLight(0xaaaaff, 0x806060, 0.2);
      var pointLight = new THREE.PointLight('#FAFAFA', 1, 100);

      this.scene.add(hemiLight);
      this.scene.add( pointLight );
      this.scene.add( directionalLight );


    },
    animate: function() {
        requestAnimationFrame(this.animate);
        this.controls.update();
        this.renderer.render(this.scene, this.camera);
    },

    loadModel: function(path) {
      console.log("load model");
      var loader = new ColladaLoader();
      loader.load( path, ( collada ) => {
        console.log(path);
        // var dae;
        var dae = collada.scene;
        dae.position.y = -0.5;
        dae.scale.x = 0.5;
        dae.scale.y = 0.5;
        dae.scale.z = 0.5;
        this.scene.add(dae);
      });
    },
  },
  mounted() {
    this.$http.get(this.apiEndPoint)
        .then(function(data){
            let bodyText = data.bodyText;
            let re = new RegExp('<li><a href="(.*)"');
            var functions = this;
            bodyText.replace(/<li><a href="(.*?)"/g, function(g0,g1){
              var x = g1.substring(g1.indexOf(".")+1)
              if (x=='dae'){
                functions.nameList.push(g1);
              }
            });
        })
  },
}
</script>

<style>
.container-fluid{
  background: 0xffffff;
}
</style>
