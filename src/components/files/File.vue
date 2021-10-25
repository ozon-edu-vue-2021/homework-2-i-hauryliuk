<template>
  <div
    class="file"
    :class="{
      file_selected: isSelected,
    }"
    :style="cssStyles"
    @click="onSelect"
  >
    <span class="file__icon">
      <base-icon width="100%" height="100%" :iconType="file.type">
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
    oneLevelIndentSize: {
      type: Number,
      default: 20,
    },
  },
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
      return this.oneLevelIndentSize * this.nestingLevel;
    },
    cssStyles() {
      return {
        'padding-left': `${this.indentSize}px`,
      };
    },
  },
  methods: {
    onSelect() {
      this.isSelected = !this.isSelected;
    },
  },
});
</script>

<style scoped>
.file {
  position: relative;
  padding: 2px;
  user-select: none;
  opacity: 0.7;
}
.file:hover {
  background-color: rgb(240, 240, 240);
  opacity: 1;
  cursor: pointer;
}
.file_selected {
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