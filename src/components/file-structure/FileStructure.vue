<template>
  <div class="file-structure">
    <div class="path">{{selectedFile ? selectedFile.pathToFile : null}}</div>
    <directory :directory="dirData" :nestingLevel="1" :pathTo="''" ref="dir"></directory>
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
        indentLevelLimit: 5,
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
  },
  mounted() {
    this.$refs.dir.$refs['kb-navigabable'].focus();
  }
};
</script>

<style scoped>
.path {
  min-height: 1.2em;
  margin-bottom: 20px;
  font-size: 0.8em;
}
</style>