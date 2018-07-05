<template>
  <div id="container">
    <div id="Stats-output"></div>
  </div>
</template>

<script>
let THREE = require('three');
const Stats = require('stats-js');
import * as dat from 'dat.gui';
  export default {
    data () {
      return {
        // msg: ""
      }
    },
    methods: {
      init: function() {
        console.log("asdfjsdalkf");
        this.scene = new THREE.Scene();

        this.camera = new THREE.PerspectiveCamera(60, window.innerWidth / window.innerHeight, 0.1, 2000);

        this.renderer = new THREE.WebGLRenderer({antialias: true});
        this.renderer.setClearColor(0xEEEEEE, 1.0);
        this.renderer.setSize(window.innerWidth, window.innerHeight);
        this.renderer.shadowMapEnabled = true;

        this.axes = new THREE.AxesHelper( 20 );
        this.scene.add(this.axes);

        // GROUND
        this.geometry = new THREE.PlaneGeometry(10, 10);
        this.material = new THREE.MeshBasicMaterial({color:0x666666});
        this.plane = new THREE.Mesh(this.geometry, this.material);
        this.scene.add(this.plane);

        // BLACK PLANE
        this.geometry1 = new THREE.PlaneGeometry(4, 3);
        this.material1 = new THREE.MeshBasicMaterial({color:'#000000'});
        this.plane1 = new THREE.Mesh(this.geometry1, this.material1);
        this.plane1.position.x=-19;
        this.plane1.position.y=0;
        this.plane1.position.z=0;
        this.scene.add(this.plane1);
        console.log(this.plane1);

        this.meshMaterial = new THREE.MeshBasicMaterial({color:0x7777ff});
        this.meshMaterial.visible = true;
        console.log(this.meshMaterial);

        this.controls = new function () {
            this.x = 0;
            this.y = 0;
            this.z = 0;
            // this.intensity = 1;
        };
        var gui = new dat.GUI();
        gui.add(this.controls, 'x').min(-19).max(19).step(0.25);
        gui.add(this.controls, 'y').min(-20).max(20).step(0.25);
        gui.add(this.controls, 'z').min(-20).max(7).step(0.25);

        this.container = document.getElementById('container');
        this.container.appendChild(this.renderer.domElement);

        // this.scene.add(this.cube);

        this.camera.position.x = 0;
        this.camera.position.y = -10;
        this.camera.position.z = 1;
        this.camera.lookAt(this.scene.position);
        // this.camera.lookAt(new THREE.Vector3(10, 0, 0));

        this.ambientLight = new THREE.AmbientLight(0x0c0c0c);
        this.scene.add(this.ambientLight);
        this.renderScene();
      },
      renderScene: function(){
        this.stats.update();

        this.plane1.position.x =this.controls.x;
        this.plane1.position.y =this.controls.y;
        this.plane1.position.z =this.controls.z;

        requestAnimationFrame(this.renderScene);
        this.renderer.render(this.scene, this.camera);

      },

      initStats: function() {
        this.stats = new Stats();
        this.stats.setMode(0);
        this.stats.domElement.style.position = 'absolute';
        this.stats.domElement.style.left = '0px';
        this.stats.domElement.style.top = '0px';
        this.statID = document.getElementById('Stats-output');
        this.statID.appendChild(this.stats.domElement);
        return this.stats;
      },
    },
    mounted() {
      this.stats = this.initStats();
      this.init();
    },
  }
</script>

<style>

</style>


