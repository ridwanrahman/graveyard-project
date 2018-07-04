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
      nameList: [],
      dae: {}
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
      this.scene.background = new THREE.Color( 0xf0f0f0 );
      this.scene.fog = new THREE.FogExp2( 0xf0f0f0, 0.030 );

      var container = document.getElementById('container');
      this.camera = new THREE.PerspectiveCamera(45, window.innerWidth / window.innerHeight, 1, 1000);
      this.camera.setFocalLength("35");
      this.camera.position.set(4.5,3.5,10);

      this.camera.lookAt(this.scene.position);
      // this.camera.lookAt(new THREE.Vector3( 0, 3, 0 ));

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

      // var light = new THREE.DirectionalLight(0x505050,1);

      // var directionalLight = new THREE.DirectionalLight( 0xffffff, 0.8 );
      // var hemiLight = new THREE.HemisphereLight(0xaaaaff, 0x806060, 0.2);
      // var pointLight = new THREE.PointLight('#FAFAFA', 1, 100);

      // this.scene.add(hemiLight);
      // this.scene.add( pointLight );
      // this.scene.add( directionalLight );
      var ambientLight = new THREE.AmbientLight(0xffffff, 0.5 );
      this.scene.add(ambientLight);

      // this.directionalLight = new THREE.DirectionalLight( 0xffffff, 0.2 );
      // this.directionalLight.position.set( 1, 1, 0 );
      // this.scene.add(this.directionalLight);

      // DIRECTIONAL LIGHT
      this.directionalLight = new THREE.DirectionalLight( 0xffffff, 0.2 );
	    this.directionalLight.angle = 1.05;
	    this.directionalLight.position.set( 4, 10, 2 );
	    this.directionalLight.castShadow = true;
	    this.directionalLight.penumbra = 1;
	    this.directionalLight.decay = 1;
	    this.directionalLight.distance = 50;

      this.directionalLight2 = new THREE.DirectionalLight( 0xffffff, 0.2 );
	    this.directionalLight2.angle = 1.05;
	    this.directionalLight2.position.set( -4, 10, 2 );
	    this.directionalLight2.castShadow = true;
	    this.directionalLight2.penumbra = 1;
	    this.directionalLight2.decay = 1;
	    this.directionalLight2.distance = 50;

      this.directionalLight3 = new THREE.DirectionalLight( 0xffffff, 0.4 );
	    this.directionalLight3.angle = 0.65;
	    this.directionalLight3.position.set( 4, 4, -5 );
	    this.directionalLight3.castShadow = false;
	    this.directionalLight3.penumbra = 1;
	    this.directionalLight3.decay = 1;
	    this.directionalLight3.distance = 50;

      this.directionalLight4 = new THREE.DirectionalLight( 0xffffff, 0.4 );
	    this.directionalLight4.angle = 0.65;
	    this.directionalLight4.position.set( -4, 5, -5 );
	    this.directionalLight4.castShadow = false
	    this.directionalLight4.penumbra = 1;
	    this.directionalLight4.decay = 1;
	    this.directionalLight4.distance = 50;

      this.directionalLight5 = new THREE.DirectionalLight( 0xffffff, 0.4 );
	    this.directionalLight5.angle = 1.05;
	    this.directionalLight5.position.set( -1, 0.5, 3 );
	    this.directionalLight5.castShadow = true;
	    this.directionalLight5.penumbra = 1;
	    this.directionalLight5.decay = 1;
	    this.directionalLight5.distance = 50;

      this.scene.add( this.directionalLight );
      this.scene.add(this.directionalLight2);
      this.scene.add( this.directionalLight3 );
      this.scene.add( this.directionalLight4 );
      this.scene.add( this.directionalLight5 );
      // DIRECTIONAL LIGHT

      // var material = new THREE.MeshPhongMaterial( { color: 0x808080, dithering: true } );
	    // var geometry = new THREE.BoxGeometry( 2000, 1, 2000 );
      // var mesh = new THREE.Mesh( geometry, material );
      // mesh.position.set( 0, -0.5, 0 );
      // mesh.receiveShadow = true;
      // this.scene.add( mesh );


    },
    renderModel: function() {
      this.renderer.render(this.scene, this.camera);

    },

    animate: function() {
        requestAnimationFrame(this.animate);
        this.controls.update();
        this.renderModel();

    },

    loadModel: function(path) {
      console.log("load model");
      var loader = new ColladaLoader();

      loader.load( path, ( collada ) => {
        this.dae = collada.scene;
        console.log(typeof (this.dae))
        this.dae.position.y = -0.5;
        this.dae.scale.x = 0.5;
        this.dae.scale.y = 0.5;
        this.dae.scale.z = 0.5;
        this.scene.add(this.dae);
      });
    },
  },
  mounted() {
    console.log("asafsdf")
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
            console.log(22, nameList);
        })
  },
}
</script>

<style>
.container-fluid{
  background: 0xffffff;
}
</style>
