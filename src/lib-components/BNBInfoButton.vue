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

<style lang="scss">
.infobutton {
  @apply bg-transparent text-gray-700 font-black border border-gray-300 rounded-full ml-1 mt-1 cursor-pointer outline-none;
  width: 24px;
  height: 24px;
  line-height: 18px;
  padding: 2px;
  display: inline;
  position: relative;
  top: 2px;

  svg {
    @apply relative;
    left: -3px;
    top: -3px;
  }
  &:focus {
    @apply outline-none;
  }
}
</style>
