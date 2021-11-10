<template>
  <svg xmlns="http://www.w3.org/2000/svg"
    :fill="iconColor"
    :width="width"
    :height="height"
    viewBox="0 0 24 24"
    :aria-labelledby="iconName"
    role="presentation"
  >
    <slot></slot>
  </svg>
</template>

<script>
export default {
  name: 'BaseIcon',
  props: {
    iconType: {
      type: String,
      default: 'file',
    },
    width: {
      type: [Number, String],
      default: '100%',
    },
    height: {
      type: [Number, String],
      default: '100%',
    },
    iconColor: {
      type: String,
      default: 'currentColor',
    },
  },
  computed: {
    iconName() {
      return `${this.iconType} icon`;
    },
  },
};
</script>
