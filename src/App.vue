<template>
  <div id="app">
    <!-- <threedconfigurator></threedconfigurator> -->
    <!-- <graveconfigurator></graveconfigurator> -->
    <!-- <graveconfigurator2></graveconfigurator2> -->
    <prac></prac>
  </div>
</template>

<script>
  import threedconfigurator from './components/threedconfigurator.vue';
  import graveconfigurator from './components/graveconfigurator.vue';
  import graveconfigurator2 from './components/graveconfigurator2.vue';
  import prac from './components/prac.vue';

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

      }
  }
</script>

<style>

</style>
