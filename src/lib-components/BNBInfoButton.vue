<template>
  <div class="bnb-info-button">
    <button @click="setInfoState(true)" class="info-button">
      <bnb-icon :path-info="icons.mdiInformation" display="inline" />
    </button>
    <bnb-modal-dialog
      :ref="infoRef"
      :title="title"
      :init-is-open="infoStateOpen"
      :hide-cancel="true"
      role="dialog"
    >
      <slot />
    </bnb-modal-dialog>
  </div>
</template>

<script>
import { mdiInformation } from './mdi.js';
import BNBIcon from './BNBIcon.vue';
import BNBModalDialog from './BNBModalDialog.vue';

export default {
  name: 'BNBInfoButton',
  components: {
    'bnb-icon': BNBIcon,
    'bnb-modal-dialog': BNBModalDialog,
  },
  props: {
    infoRef: {
      type: String,
      default: 'infoRef',
    },
    title: {
      type: String,
      default: 'Title',
    },
  },

  data() {
    return {
      icons: { mdiInformation },
      infoStateOpen: false,
    };
  },

  methods: {
    setInfoState(bool) {
      this.$refs[this.infoRef].isOpen = bool;
    },
  },
};
</script>

<style>
.bnb-info-button {
  display: inline;
}
.info-button {
  box-sizing: border-box;
  display: inline;
  position: relative;
  top: 2px;
  width: 24px;
  height: 24px;
  line-height: 18px;
  padding: 2px;
  background-color: transparent;
  color: #4a5568;
  font-weight: 900;
  border: 1px solid #4a5568;
  border-radius: 100%;
  cursor: pointer;
  outline: 2px solid transparent;
  outline-offset: 2px;
}

.info-button svg {
  position: relative;
  left: -3px;
  top: -3px;
}

.info-button:focus {
  outline: 2px solid transparent;
  outline-offset: 2px;
}
</style>
