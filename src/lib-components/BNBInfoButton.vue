<template>
  <div class="info-wrapper inline font-normal">
    <div class="info-button inline">
      <button @click="setInfoState(true)" class="infobutton">
        <bnb-icon :path-info="icons.mdiInformation" display="inline" />
      </button>
    </div>
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
import { mdiInformation } from '@mdi/js/mdi.js';
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
.infobutton {
  width: 24px;
  height: 24px;
  line-height: 18px;
  padding: 2px;
  display: inline;
  position: relative;
  top: 2px;
  box-sizing: border-box;
  background-color: transparent;
  color: #4a5568;
  color: rgba(74, 85, 104, 1);
  font-weight: 900;
  border: 1px solid #e2e8f0;
  border-radius: 100%;
  cursor: pointer;
  outline: 2px solid transparent;
  outline-offset: 2px;
}

.infobutton svg {
  position: relative;
  left: -3px;
  top: -3px;
}

.infobutton:focus {
  outline: 2px solid transparent;
  outline-offset: 2px;
}
</style>
