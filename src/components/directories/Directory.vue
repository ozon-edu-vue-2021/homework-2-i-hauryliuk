<template>
  <div class="directory">
    <div
      class="directory__heading"
      @click="toggleContentVisibility"
      :style="indentRule"
    >
      <span class="directory__icon"></span>
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
import File from '../files/File.vue'

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

</style>
