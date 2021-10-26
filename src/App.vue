<template>
  <div id="app">
    <h1>{{heading}}</h1>
    <section class="container">
      <directory :directory="dirData" :nestingLevel="1"></directory>
    </section>
  </div>
</template>

<script>
import Directory from './components/directories/Directory.vue';
import dirData from '../public/static/node_modules.json';

export default {
  name: 'App',
  data() {
    return {
      heading: 'Directory component',
      dirData,
    };
  },
  components: {
    Directory,
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
