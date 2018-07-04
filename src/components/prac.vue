<template>
<div id="container">
  <div id="Stats-output"></div>

</div>

</template>

<script>
  const THREE = require('three');
  const Stats = require('stats-js');
  import * as dat from 'dat.gui';

  export default {
    data() {
      return {
        msg: "prac",
        step: 0,
      }
    },
    methods: {
      init: function() {
        console.log("asdfds");
        this.scene = new THREE.Scene();

        this.camera = new THREE.PerspectiveCamera(45, window.innerWidth / window.innerHeight, 0.1, 1000);

        this.renderer = new THREE.WebGLRenderer({antialias: true});
        this.renderer.setClearColor(0xEEEEEE, 1.0);
        this.renderer.setSize(window.innerWidth, window.innerHeight);
        this.renderer.shadowMapEnabled = true;

        this.axes = new THREE.AxesHelper( 20 );
        this.scene.add(this.axes);

        this.planeGeometry = new THREE.PlaneGeometry(60,20,1,1);
        this.planeMaterial = new THREE.MeshLambertMaterial({color: 0xcccccc});
        this.plane = new THREE.Mesh(this.planeGeometry, this.planeMaterial);

        this.plane.rotation.x=-0.5*Math.PI;
        this.plane.position.x = 15;
        this.plane.position.y = 0;
        this.plane.position.z = 0;
        this.plane.receiveShadow = true;

        this.scene.add(this.plane);

        this.cubeGeometry = new THREE.CubeGeometry(4,4,4);
        this.cubeMaterial = new THREE.MeshLambertMaterial({color: 0xff0000});
        this.cube = new THREE.Mesh(this.cubeGeometry, this.cubeMaterial);

        this.cube.position.x = -4;
        this.cube.position.y = 3;
        this.cube.position.z = 0;
        this.cube.castShadow = true;

        this.scene.add(this.cube);

        this.sphereGeometry = new THREE.SphereGeometry(4,20,20);
        this.sphereMaterial = new THREE.MeshLambertMaterial({color: 0x7777ff});
        this.sphere = new THREE.Mesh(this.sphereGeometry, this.sphereMaterial);
        this.sphere.castShadow = true;

        this.sphere.position.x = 20;
        this.sphere.position.y = 4;
        this.sphere.position.z = 2;

        this.scene.add(this.sphere);

        this.camera.position.x = -30;
        this.camera.position.y = 40;
        this.camera.position.z = 30;
        this.camera.lookAt(this.scene.position);

        this.spotLight = new THREE.SpotLight( 0xffffff );
        this.spotLight.position.set( -40, 60, -10 );
        this.scene.add( this.spotLight );
        this.spotLight.castShadow = true;

        var container = document.getElementById('container');
        container.appendChild(this.renderer.domElement);

        this.renderScene();

      },
      renderScene: function() {
        // console.log(55,this.stats);
        this.stats.update();
        // if(this.stats){
        // }

        this.cube.rotation.x += 0.02;
        this.cube.rotation.y += 0.02;
        this.cube.rotation.z += 0.02;

        this.step += 0.04;
        this.sphere.position.x = 20+( 10*(Math.cos(this.step)));
        this.sphere.position.y = 2 +( 10*Math.abs(Math.sin(this.step)));

        requestAnimationFrame(this.renderScene);
        this.renderer.render(this.scene, this.camera);
      },

      controls: function() {
        var gui = new dat.GUI();
        gui.add(this.controls, 'rotationSpeed',0,0.5);
        gui.add(this.controls, 'bouncingSpeed',0,0.5);
      },

      initStats: function() {
        this.stats = new Stats();
        console.log("stats initiated");
        this.stats.setMode(0);

        this.stats.domElement.style.position = 'absolute';
        this.stats.domElement.style.left = '0px';
        this.stats.domElement.style.top = '0px';
        this.statID = document.getElementById('Stats-output');
        this.statID.appendChild(this.stats.domElement);

        return this.stats;
      }
    },
    mounted() {
      this.stats=this.initStats();
      this.init();
      console.log(this.stats.update());
      console.log(22,this.stats);

    },
}
</script>

<style>

</style>


