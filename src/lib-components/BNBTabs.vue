<template>
  <nav class="bnb-tabs">
    <button
      v-ripple.400="'rgba(0, 0, 0, 0.02)'"
      v-for="tab in tabs"
      :key="tab.title"
      :ref="tab.value"
      type="button"
      class="bnb-tabs__item"
      :class="[{ 'bnb-tabs__item_active': tab.value === currentTab }]"
      :style="`color: ${textColor};`"
      @click="handleClick(tab.value)"
    >
      {{ tab.title }}
    </button>

    <div class="bnb-tabs__active-line" />
  </nav>
</template>

<script>
import Vue from 'vue';
import Ripple from './ripple.js';
Vue.directive('ripple', Ripple);
//
// Based on https://www.npmjs.com/package/vue-tabs-with-active-line
//
export default {
  name: 'BNBTabs',
  props: {
    textColor: {
      type: String,
      default: '#ffffff',
    },
    underlineColor: {
      type: String,
      default: '#ffffff',
    },
    tabs: {
      type: Array,
      default: () => {
        return [];
      },
    },
    initialCurrentTab: {
      type: String,
      default: '',
    },
  },
  data: () => ({
    activeLineWidth: 0,
    activeLineOffset: 0,
    currentTab: '',
  }),
  mounted() {
    this.moveActiveLine(this.initialCurrentTab);
  },
  methods: {
    handleClick(value) {
      this.$emit('onClick', value);
      this.moveActiveLine(value);
    },
    moveActiveLine(newValue) {
      this.currentTab = newValue;

      if (!this.$refs || !this.$refs[newValue] || !this.$refs[newValue][0]) {
        return;
      }
      const element = this.$refs[newValue][0];
      const tabActiveLine = document.querySelector('.bnb-tabs__active-line');

      this.activeLineWidth = element.clientWidth;
      this.activeLineOffset = element.offsetLeft;

      tabActiveLine.style.backgroundColor = this.underlineColor;
      tabActiveLine.style.width = `${this.activeLineWidth}px`;
      tabActiveLine.style.transform = `translateX(${this.activeLineOffset}px)`;
    },
  },
};
</script>

<style scoped>
.bnb-tabs {
  position: relative;
  margin: 0 auto;
  padding: 0;
}

.bnb-tabs__item {
  display: inline-block;
  position: relative;
  top: 0;
  left: 0;
  margin: 0;
  padding: 2rem 1rem 1.5rem 1rem;
  letter-spacing: 1.4px;
  line-height: 1.25rem;
  text-decoration: none;
  font-size: 13px;
  border: none;
  opacity: 0.65;
  /* color: rgba(255, 255, 255, 0.65); */
  background-color: transparent;
  border-bottom: 2px solid transparent;
  cursor: pointer;
  overflow: hidden;
  z-index: 1;
}
.bnb-tabs__item_active {
  color: rgba(255, 255, 255, 0.8);
}
.bnb-tabs__item:hover {
  opacity: 1;
  /* color: rgba(255, 255, 255, 1); */
  background-color: rgba(255, 255, 255, 0.1);
  border-bottom: 2px solid transparent;
}
.bnb-tabs__item:focus {
  outline: none;
  opacity: 0.9;
  border-bottom: 2px solid transparent;
  /* color: rgba(255, 255, 255, 0.9); */
}

.bnb-tabs__active-line {
  /* background-color: white; */
  position: absolute;
  bottom: 0;
  left: 0;
  height: 2px;
  width: 0;
  transform: translateX(0);
  transition: transform 0.4s ease-in-out, width 0.4s ease-in-out;
}
</style>
