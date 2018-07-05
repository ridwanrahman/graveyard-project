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

      handleControlChange: function(e) {
          this.pointLight.intensity = e;
      },
      init: function() {
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

        this.meshMaterial = new THREE.MeshBasicMaterial({color:0x7777ff});
        this.meshMaterial.visible = false;

        console.log(11,this.meshMaterial);

        // this.spotLight = new THREE.SpotLight( 0xffffff );
        // this.spotLight.position.set( -40, 60, -10 );
        // this.scene.add( this.spotLight );
        // this.spotLight.castShadow = true;

        this.pointColor = '#ffffff';
        this.spotLight = new THREE.SpotLight(this.pointColor);
        this.spotLight.position.set(-40, 60, 10);
        this.spotLight.castShadow = true;
        this.spotLight.target = this.plane;
        this.scene.add(this.spotLight);

        // this.ambiColor = "#0c0c0c";
        // this.ambientLight = new THREE.AmbientLight(this.ambiColor);
        // this.scene.add(this.ambientLight);

        // this.pointColor = "#ccffcc";
        // this.pointLight = new THREE.PointLight(this.pointColor);
        // this.pointLight.distance = 100;
        // this.pointLight.intensity = 2.4;
        // this.scene.add(this.pointLight);

        // this.hemiLight = new THREE.HemisphereLight(0x0000ff, 0x00ff00, 0.6);
        // this.hemiLight.position.set(0, 500, 0);
        // this.scene.add(this.hemiLight);

        var container = document.getElementById('container');
        container.appendChild(this.renderer.domElement);

        var step = 0;

        this.controls = new function () {
            this.rotationSpeed = 0.02;
            this.bouncingSpeed = 0.03;
            this.intensity = 1;
        };
        var that=this;
        var ColorText = function() {
          this.ambientColor = that.ambiColor;
        };

        var gui = new dat.GUI();
        var text = new ColorText();
        gui.add(this.controls, 'rotationSpeed', 0, 0.5);
        gui.add(this.controls, 'bouncingSpeed', 0, 0.5);
        console.log(66,gui.add(this.controls, 'intensity', 0.3));
        // gui.add(this.controls, 'intensity', 0.3).onChange(this.handleControlChange.bind(this));
        // gui.add(this.controls, 'intensity', 0.3).onChange((e)=>{
        //   this.pointLight.intensity = e;
        // });

        // gui.addColor(text, 'ambientColor').onChange((e) => {
        //   var color = new THREE.Color(e)
        //   this.ambientLight.color = color;
        // });

        this.renderScene();

      },
      renderScene: function() {
        this.stats.update();

        this.cube.rotation.x += this.controls.rotationSpeed;
        this.cube.rotation.y += this.controls.rotationSpeed;
        this.cube.rotation.z += this.controls.rotationSpeed;

        this.step += this.controls.bouncingSpeed;
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
    },
}
</script>

<style>

</style>


