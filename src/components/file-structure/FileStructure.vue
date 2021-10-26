<template>
  <div class="file-structure" @file-select="fileSelectHandler">
    <directory :directory="dirData" :nestingLevel="1"></directory>
  </div>
</template>

<script>
import data from '../../../public/static/node_modules.json';
import Directory from '../directories/Directory.vue';

export default {
  name: 'FileStructure',
  data() {
    return {
      dirData: data,
      selectedFile: null,
    };
  },
  components: {
    Directory,
  },
  provide() {
    return {
      fileSelectHandler: this.fileSelectHandler,
    };
  },
  methods: {
    fileSelectHandler(file) {
      if (this.selectedFile) {
        this.selectedFile.isSelected = false;
      }
      this.selectedFile = file;
    }
  }
};
</script>

<style scoped>

</style>