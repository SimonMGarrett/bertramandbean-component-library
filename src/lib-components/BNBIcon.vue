<template>
  <svg
    xmlns="http://www.w3.org/2000/svg"
    :viewBox="viewBox"
    :height="height"
    :width="width"
    role="img"
    aria-hidden="true"
    class="bnb-icon"
    :style="`display: ${display}`"
  >
    <path :d="pathInfo" />
  </svg>
</template>

<script>
import Vue from 'vue';

export default Vue.extend({
  name: 'BNBIcon',
  // NOTE: color is set via the 'fill: currentColor' in the style
  props: {
    pathInfo: {
      type: String,
      default: '',
    },
    height: {
      type: String,
      default: '24',
    },
    width: {
      type: String,
      default: '24',
    },
    viewBox: {
      type: String,
      default: '0 0 24 24',
    },
    display: {
      type: String,
      default: 'block',
    },
  },
});
</script>

<style scoped>
svg {
  border: none;
  fill: currentColor;
  font-size: inherit;
  padding: 0;
  margin: 0;
  border: none;
}
</style>
