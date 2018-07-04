<template>
  <div id="container" class="container-fluid">

    <div class="mod-select">
        choose a model :
        <select id="selectBox">
                <option selected="true" disabled="disabled">Select Value</option>
                <option value="http://localhost:8000/DG-1-20702-dezemoetwerken.dae">Option #1</option>
                <option value="http://localhost:8000/DG-02-21828-DG-02.dae">Option #2</option>
                <option value="http://localhost:8000/DG-03-60489-DG-03.dae">Option #3</option>
                <option value="http://localhost:8000/DG-04-46244-dg-04.dae">Option #4</option>
                <option value="http://localhost:8000/DG-09-49263-dg-09.dae">Option #5</option>
                <option value="http://localhost:8000/DG-10-99656-DG-10.dae">Option #6</option>
                <option value="http://localhost:8000/DK-01-A-39204-DK-01-A.dae">Option #7</option>
                <option value="http://localhost:8000/DK-01-A-44135-DK-01-A.dae">Option #8</option>
        </select>
        <button id="stopAnimation" value=""></button>
    </div>

  </div>
</template>

<script>
import * as Three from 'three'
export default {
  name: 'app',
  data () {
    return {
      // msg: 'Welcome to Your Vue.js App'
      camera: null,
      scene: null,
      renderer: null,
      mesh: null
    }
  },

  methods: {
    init: function() {
        var container = document.getElementById('container');

        this.camera = new Three.PerspectiveCamera(70, window.innerWidth/window.innerHeight, 0.01, 10);
        this.camera.position.z = 1;

        this.scene = new Three.Scene();

        var geometry = new Three.BoxGeometry(0.2, 0.2, 0.2);
        var material = new Three.MeshNormalMaterial();

        this.mesh = new Three.Mesh(geometry, material);
        this.scene.add(this.mesh);

        this.renderer = new Three.WebGLRenderer({antialias: true});
        this.renderer.setSize(window.innerWidth, window.innerHeight);
        container.appendChild(this.renderer.domElement);

    },
    animate: function() {
        requestAnimationFrame(this.animate);
        this.mesh.rotation.x += 0.01;
        this.mesh.rotation.y += 0.02;
        this.renderer.render(this.scene, this.camera);
    }
  },
  mounted() {
      this.init();
      this.animate()
  }
}
</script>

<style>

</style>
