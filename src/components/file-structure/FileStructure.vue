<template>
  <div class="file-structure">
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
      indentParams: {
        oneLevelIndentSize: 20,
        indentLevelLimit: 3,
      },
    };
  },
  components: {
    Directory,
  },
  provide() {
    return {
      fileSelectHandler: this.fileSelectHandler,
      indentParams: this.indentParams,
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