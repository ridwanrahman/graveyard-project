<template>
<div id="container">

</div>

</template>

<script>
  const THREE = require('three');

  export default {
    data() {
      return {
        msg: "prac"
      }
    },
    methods: {
      init: function() {
        console.log("asdfds");
        this.scene = new THREE.Scene();

        this.camera = new new THREE.PerspectiveCamera(45, window.innerWidth / window.innerHeight, 0.1, 1000);

        this.renderer = new THREE.WebGLRenderer();
        this.renderer.setClearColorHex(0xEEEEEE);
        this.renderer.setSize(window.innerWidth, window.innerHeight);

        this.axes = new THREE.AxisHelper( 20 );
        this.scene.add(this.axes);

        this.planeGeometry = new THREE.PlaneGeometry(60,20,1,1);
        this.planeMaterial = new THREE.MeshBasicMaterial({color: 0xcccccc});
        this.plane = new THREE.Mesh(this.planeGeometry, this.planeMaterial);

        this.plane.rotation.x=-0.5*Math.PI;
        this.plane.position.x = 15;
        this.plane.position.y = 0;
        this.plane.position.z = 0;

        this.scene.add(this.plane);

        this.cubeGeometry = new THREE.CubeGeometry(4,4,4);
        this.cubeMaterial = new THREE.MeshBasicMaterial({color: 0xff0000});
        this.cube = new THREE.Mesh(this.cubeGeometry, this.cubeMaterial);

        this.cube.position.x = -4;
        this.cube.position.y = 3;
        this.cube.position.z = 0;

        this.scene.add(this.cube);

        this.sphereGeometry = new THREE.SphereGeometry(4,20,20);
        this.sphereMaterial = new THREE.MeshBasicMaterial({color: 0x7777ff});
        this.sphere = new THREE.Mesh(this.sphereGeometry, this.sphereMaterial);







      }
    },
    mounted() {
      this.init();
    },
}
</script>

<style>

</style>


