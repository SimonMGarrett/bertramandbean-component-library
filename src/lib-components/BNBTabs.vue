<template>
  <nav class="bnb-tabs" :class="wrapperClass">
    <button
      v-for="tab in tabs"
      :ref="tab.value"
      :key="tab.title"
      class="bnb-tabs__item"
      type="button"
      :class="[{ 'bnb-tabs__item_active': tab.value === currentTab }]"
      :disabled="tab.disabled || false"
      @click="handleClick(tab.value)"
    >
      {{ tab.title }}
    </button>

    <div class="bnb-tabs__active-line" />
  </nav>
</template>

<script>
//
// Based on https://www.npmjs.com/package/vue-tabs-with-active-line
//
export default {
  name: 'BNBTabs',
  props: {
    wrapperClass: {
      type: String,
      required: false,
      default: '',
    },
    tabs: {
      type: Array,
      default: () => {
        return [];
      },
    },
    currentTab: {
      type: String,
      default: '',
    },
  },
  data: () => ({
    activeLineWidth: 0,
    activeLineOffset: 0,
    newTab: '',
  }),
  watch: {
    currentTab(newCurrentTab) {
      if (this.newTab === newCurrentTab) {
        return;
      }
      this.moveActiveLine(newCurrentTab);
      this.newTab = newCurrentTab;
    },
    // updated () {
    //   this.moveActiveLine(this.currentTab)
    // },
  },
  mounted() {
    this.moveActiveLine(this.currentTab);
    this.newTab = this.currentTab;
  },
  methods: {
    handleClick(value) {
      this.$emit('onClick', value);
      this.moveActiveLine(value);

      this.newTab = value;
    },
    moveActiveLine(newValue) {
      if (!this.currentTab) {
        return;
      }

      if (!this.$refs || !this.$refs[newValue] || !this.$refs[newValue][0]) {
        return;
      }
      const element = this.$refs[newValue][0];
      const tabActiveLine = document.querySelector('.bnb-tabs__active-line');

      this.activeLineWidth = element.clientWidth;
      this.activeLineOffset = element.offsetLeft;

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
  color: rgba(255, 255, 255, 0.65);
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
  color: rgba(255, 255, 255, 1);
  background-color: rgba(255, 255, 255, 0.1);
  border-bottom: 2px solid transparent;
}
.bnb-tabs__item:focus {
  outline: none;
  border-bottom: 2px solid transparent;
  color: rgba(255, 255, 255, 0.9);
}

.bnb-tabs__active-line {
  background-color: white;
  position: absolute;
  bottom: 0;
  left: 0;
  height: 2px;
  width: 0;
  transform: translateX(0);
  transition: transform 0.4s ease-in-out, width 0.4s ease-in-out;
}
</style>
