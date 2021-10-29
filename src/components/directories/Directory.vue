<template>
  <div class="directory">
    <div
      class="directory__heading"
      :class="{
        directory__heading_expanded: isExpanded,
      }"
      :style="indentRule"
      tabindex="0"
      ref='kb-navigabable'
      @click="toggleContentVisibility"
      @keyup.enter="handleEnterKeystroke"
      @keydown.prevent.up="handleUpArrowKeystroke"
      @keydown.prevent.down="handleDownArrowKeystroke"
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
            :pathTo="pathTo.length
              ? `${pathTo}/${directory.name}`
              : `${directory.name}`"
          ></directory>
          <file
            v-else :file="entry"
            :key="`${entry.name}-${idx}`"
            :nestingLevel="nextLevel"
            :pathTo="`${pathTo}/${directory.name}`"
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
    pathTo: {
      type: String,
      required: true,
    },
  },
  components: {
    File,
    BaseIcon,
    DirIcon,
    ArrowUpIcon,
  },
  inject: ['indentParams'],
  computed: {
    nextLevel() {
      return this.nestingLevel + 1;
    },
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
  },
  methods: {
    toggleContentVisibility() {
      if (!this.isViewed) {
        this.isViewed = true;
      }
      this.isExpanded = !this.isExpanded;
    },
    handleEnterKeystroke(event) {
      event.target.click();
    },
    getNavigabable(children) {
      return children.filter((item) => item.$refs['kb-navigabable']);
    },
    handleUpArrowKeystroke() {
      const navigabableSiblings = this.getNavigabable(this.$parent.$children);
      const thisIdx = navigabableSiblings.findIndex((item) => item === this);
      if (thisIdx > 0) {
        if (navigabableSiblings[thisIdx - 1].isExpanded) {
          const prevSiblingChildren = this.getNavigabable(navigabableSiblings[thisIdx - 1].$children);
          let lastChild = prevSiblingChildren[prevSiblingChildren.length - 1];
          while (lastChild.isExpanded) {
            let lastChildChildren = this.getNavigabable(lastChild.$children)
            lastChild = lastChildChildren[lastChildChildren.length - 1];
          }
          lastChild.$refs['kb-navigabable'].focus();
        } else {
          navigabableSiblings[thisIdx - 1].$refs['kb-navigabable'].focus();
        }
      } else if (this.$parent.$refs['kb-navigabable']) {
        this.$parent.$refs['kb-navigabable'].focus();
      }
    },
    handleDownArrowKeystroke() {
      if (this.isExpanded) {
        const navigabableChildren = this.getNavigabable(this.$children);
        navigabableChildren[0].$refs['kb-navigabable'].focus();
      }
      else {
        const navigabableSibling = this.getNavigabable(this.$parent.$children);
        const thisIdx = navigabableSibling.findIndex((item) => item === this);
        if (thisIdx < navigabableSibling.length - 1) {
          navigabableSibling[thisIdx + 1].$refs['kb-navigabable'].focus();
        } else if (this.$parent.$refs['kb-navigabable']) {
          let parent = this.$parent;
          let parentNavigabableSiblings = this.getNavigabable(parent.$parent.$children)
          let parentIdx = parentNavigabableSiblings.findIndex((item) => item === parent);
          while (parentIdx === parentNavigabableSiblings.length - 1 && parent.$refs['kb-navigabable']) {
            parent = parent.$parent;
            parentNavigabableSiblings = this.getNavigabable(parent.$parent.$children)
            parentIdx = parentNavigabableSiblings.findIndex((item) => item === parent);
          }
          parentNavigabableSiblings[parentIdx + 1]?.$refs['kb-navigabable'].focus();
        }
      }
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
