<template>
  <div id="app">
    <h1>{{heading}}</h1>
    <section class="container">
      <file-structure></file-structure>
    </section>
  </div>
</template>

<script>
import FileStructure from './components/file-structure/FileStructure.vue';

export default {
  name: 'App',
  data() {
    return {
      heading: 'Directory component',
    };
  },
  components: {
    FileStructure,
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
