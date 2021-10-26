<template>
  <div class="directory">
    <div
      class="directory__heading"
      :class="{
        directory__heading_expanded: isExpanded,
      }"
      @click="toggleContentVisibility"
      :style="indentRule"
    >
      <span class="directory__icon">
          <base-icon :iconType="directory.type">
            <arrow-up-icon v-if="isExpanded"></arrow-up-icon>
            <dir-icon v-else></dir-icon>
          </base-icon>
      </span>
      <span class="directory__name">{{directory.name}}</span>
    </div>
    <div class="directory__content" v-show="isExpanded">
      <template v-if="isViewed">
        <template v-for="(entry, idx) in directory.contents">
          <directory
            v-if="entry.type === 'directory'"
            :directory="entry"
            :nestingLevel="nextLevel"
            :key="`${entry.name}-${idx}`"
          ></directory>
          <file
            v-else :file="entry"
            :key="`${entry.name}-${idx}`"
            :nestingLevel="nextLevel"
          ></file>
        </template>
      </template>
    </div>
  </div>
</template>

<script>
import File from '../files/File.vue';
import BaseIcon from '../icons/BaseIcon.vue';
import DirIcon from '../icons/DirIcon.vue';
import ArrowUpIcon from '../icons/ArrowUpIcon.vue';

export default {
  name: 'Directory',
  data() {
    return {
      isExpanded: false,
      isViewed: false,
      oneLevelIndentSize: 20,
    };
  },
  props: {
    directory: {
      type: Object,
      required: true,
    },
    nestingLevel: {
      type: Number,
      default: 0,
    },
  },
  components: {
    File,
    BaseIcon,
    DirIcon,
    ArrowUpIcon,
  },
  provide() {
    return {
      oneLevelIndentSize: this.oneLevelIndentSize,
    };
  },
  computed: {
    nextLevel() {
      return this.nestingLevel + 1;
    },
    indentSize() {
      return this.oneLevelIndentSize * this.nestingLevel;
    },
    indentRule() {
      return {
        'padding-left': `${this.indentSize}px`,
      };
    },
  },
  methods: {
    toggleContentVisibility() {
      if (!this.isViewed) {
        this.isViewed = true;
      }
      this.isExpanded = !this.isExpanded;
    },
  },
};
</script>

<style scoped>
.directory__heading {
  position: relative;
  padding: 2px;
  border-left: 4px solid transparent;
  user-select: none;
  opacity: 0.7;
}
.directory__heading:hover {
  background-color: rgb(240, 240, 240);
  opacity: 1;
  cursor: pointer;
}
.directory__heading_expanded {
  background-color: rgb(240, 240, 240);
  opacity: 1;
  cursor: pointer;
}
.directory__icon {
  position: absolute;
  top: 50%;
  width: 0.5em;
  height: 0.5em;
  transform: translate(-110%, -50%);
  display: flex;
  justify-content: center;
  align-items: center;
}
.directory__name {
  display: block;
  box-sizing: border-box;
  width: 100%;
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
}
</style>
