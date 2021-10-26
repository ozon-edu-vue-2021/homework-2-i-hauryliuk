<template>
  <div
    class="file"
    :class="{
      file_selected: isSelected,
    }"
    @click="onSelect"
    :style="indentRule"
  >
    <span class="file__icon">
      <base-icon :iconType="file.type">
        <link-icon v-if="file.type === 'link'"></link-icon>
        <file-icon v-else></file-icon>
      </base-icon>
    </span>
    <span class="file__name">{{file.name}}</span>
  </div>
</template>

<script>
import BaseIcon from '../icons/BaseIcon.vue';
import FileIcon from '../icons/FileIcon.vue';
import LinkIcon from '../icons/LinkIcon.vue';

export default ({
  name: 'File',
  props: {
    file: {
      type: Object,
      required: true,
    },
    nestingLevel: {
      type: Number,
      default: 0,
    },
    pathTo: {
      type: String,
      required: true,
    },
  },
  inject: ['indentParams', ['fileSelectHandler']],
  data() {
    return {
      isSelected: false,
    };
  },
  components: {
    BaseIcon,
    FileIcon,
    LinkIcon,
  },
  computed: {
    indentSize() {
      return this.nestingLevel > this.indentParams.indentLevelLimit
        ? this.indentParams.oneLevelIndentSize * this.indentParams.indentLevelLimit
        : this.indentParams.oneLevelIndentSize * this.nestingLevel;
    },
    indentRule() {
      return {
        'padding-left': `${this.indentSize}px`,
      };
    },
    pathToFile() {
      return `${this.pathTo}/${this.file.name}`;
    },
  },
  methods: {
    onSelect() {
      if(!this.isSelected) {
        this.isSelected = true;
        this.fileSelectHandler(this);
      }
    },
  },
});
</script>

<style scoped>
.file {
  position: relative;
  padding: 2px;
  border-left: 4px solid transparent;
  user-select: none;
  opacity: 0.7;
}
.file:hover {
  background-color: rgb(240, 240, 240);
  opacity: 1;
  cursor: pointer;
}
.file_selected {
  border-left-color: currentColor;
  background-color: rgb(221, 221, 221);
  opacity: 1;
}
.file_selected:hover {
  background-color: rgb(221, 221, 221);
  opacity: 1;
  cursor: default;
}
.file__icon {
  position: absolute;
  top: 50%;
  width: 0.5em;
  height: 0.5em;
  transform: translate(-110%, -50%);
  display: flex;
  justify-content: center;
  align-items: center;
}
.file__name {
  display: block;
  box-sizing: border-box;
  width: 100%;
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
}
</style>