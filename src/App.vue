<template>
  <div id="app">
    <!-- <threedconfigurator></threedconfigurator> -->
    <!-- <graveconfigurator></graveconfigurator> -->
    <!-- <graveconfigurator2></graveconfigurator2> -->
    <!-- <prac></prac> -->
    <!-- <chapter4></chapter4> -->
    <collada></collada>
  </div>
</template>

<script>
  import threedconfigurator from './components/threedconfigurator.vue';
  import graveconfigurator from './components/graveconfigurator.vue';
  import graveconfigurator2 from './components/graveconfigurator2.vue';
  import prac from './components/prac.vue';
  import chapter4 from './components/chapter4.vue';
  import collada from './components/collada.vue';

  export default {
    name: 'app',
    data () {
      return {
        title: 'Ninja App',
      }
    },
    components: {
        threedconfigurator: threedconfigurator,
        graveconfigurator: graveconfigurator,
        graveconfigurator2: graveconfigurator2,
        prac: prac,
        chapter4: chapter4,
        collada: collada

      }
  }
</script>

<style>

</style>
