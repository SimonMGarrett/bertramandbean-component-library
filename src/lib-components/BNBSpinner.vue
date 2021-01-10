<template>
  <span v-if="show" class="bnb-spinner" :style="style"></span>
</template>

<script>
export default {
  name: 'BNBSpinner',
  props: {
    color: {
      type: String,
      default: '#DF165A',
    },
    size: {
      type: Number,
      default: 14,
    },
    width: {
      type: Number,
      default: 2,
    },
    duration: {
      type: String,
      default: '1s',
    },
    show: {
      type: Boolean,
      default: false,
    },
  },
  computed: {
    style() {
      return {
        borderTopColor: this.color,
        borderWidth: `${this.width}px`,
        width: `${this.size}px`,
        height: `${this.size}px`,
        animationDuration: this.duration,
      };
    },
  },
};
</script>

<style scoped>
.bnb-spinner {
  position: relative;
  top: -2px;
  display: inline-block;
  vertical-align: middle;
  border: 2px solid #c2c2c2; /* Light grey */
  border-radius: 50%;
  /* border notch color + border width + width + height defined above */
  margin: 0;
  padding: 0;
  opacity: 1;
  transition: all 0.2s ease;
  animation: spin 1s linear infinite;
}

@keyframes spin {
  0% {
    transform: rotate(0deg);
  }

  100% {
    transform: rotate(360deg);
  }
}
</style>
