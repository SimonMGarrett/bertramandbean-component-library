<template>
  <div class="always-present-wrapper p-0 m-0">
    <transition
      enter-active-class="transition ease-out duration-300 transform"
      enter-class="opacity-0"
      enter-to-class="opacity-100"
      leave-active-class="transition ease-in duration-200 transform"
      leave-class="opacity-100"
      leave-to-class="opacity-0"
    >
      <!--
        Tailwind UI components require Tailwind CSS v1.8 and the @tailwindcss/ui plugin.
        Read the documentation to get started: https://tailwindui.com/documentation
      -->
      <div
        v-show="isOpen"
        class="fixed mt-12 md:mt-0 z-40 inset-0 overflow-y-auto"
      >
        <div
          class="flex items-end justify-center min-h-screen px-4 pb-16 text-center sm:block sm:p-0"
        >
          <!--
              Background overlay, show/hide based on modal state.

              Entering: "ease-out duration-300"
                From: "opacity-0"
                To: "opacity-100"
              Leaving: "ease-in duration-200"
                From: "opacity-100"
                To: "opacity-0"
            -->
          <div class="fixed inset-0 transition-opacity">
            <div class="absolute inset-0 bg-gray-500 opacity-75" />
          </div>

          <!-- This element is to trick the browser into centering the modal contents. (SMG removed &#8203; that followed) -->
          <span class="hidden sm:inline-block sm:align-middle sm:h-screen" />
          <!--
              Modal panel, show/hide based on modal state.

              Entering: "ease-out duration-300"
                From: "opacity-0 translate-y-4 sm:translate-y-0 sm:scale-95"
                To: "opacity-100 translate-y-0 sm:scale-100"
              Leaving: "ease-in duration-200"
                From: "opacity-100 translate-y-0 sm:scale-100"
                To: "opacity-0 translate-y-4 sm:translate-y-0 sm:scale-95"
            -->
          <div
            class="inline-block align-bottom bg-white text-gray-700 rounded-lg text-left overflow-hidden shadow-xl transform transition-all sm:my-8 sm:align-middle sm:max-w-lg sm:w-full"
            role="dialog"
            aria-modal="true"
            :aria-labelledby="ariaLabel"
          >
            <div class="as-h4 modal-title font-bold m-4 mb-8 w-4/5">
              {{ title }}
            </div>
            <bnb-icon
              class="close-icon text-gray-600"
              :path-info="icons.mdiCloseThick"
              @click.native="isOpen = false"
            />

            <div class="bg-white px-4 py-0">
              <!-- HTML body content -->
              <slot />
            </div>
            <!-- Action buttons -->
            <div
              v-show="hideButtons === false"
              class="bg-gray-100 px-4 py-3 sm:px-6 sm:flex sm:flex-row-reverse"
            >
              <span
                v-show="hideOk === false"
                class="flex w-full rounded-md shadow-sm sm:ml-3 sm:w-auto"
              >
                <button
                  type="button"
                  class="inline-flex justify-center w-full rounded-md border btn btn-bbpink"
                  @click="
                    okCallbackActive = true;
                    okCallback();
                  "
                >
                  Ok
                  <bnb-spinner :show="okCallbackActive" />
                </button>
              </span>

              <span
                v-show="hideCancel === false"
                class="mt-3 flex w-full rounded-md shadow-sm sm:mt-0 sm:w-auto"
              >
                <button
                  type="button"
                  class="inline-flex justify-center w-full rounded-md border btn btn-outline"
                  @click="isOpen = false"
                >
                  Cancel
                </button>
              </span>
            </div>
          </div>
        </div>
      </div>
    </transition>
  </div>
</template>

<script>
import Vue from 'vue';
import { mdiCloseThick } from '@mdi/js/mdi.js';
import BNBIcon from './BNBIcon.vue';
import BNBSpinner from './BNBSpinner.vue';

export default Vue.extend({
  name: 'BNBModalDialog',
  components: {
    'bnb-icon': BNBIcon,
    'bnb-spinner': BNBSpinner,
  },
  props: {
    title: {
      type: String,
      default: 'Title',
    },
    initIsOpen: {
      type: Boolean,
      default: false,
    },
    okCallback: {
      type: Function,
      default() {
        this.isOpen = false;
        this.okCallbackActive = false;
        return null; // noop
      },
    },
    hideButtons: {
      type: Boolean,
      default: false,
    },
    hideOk: {
      type: Boolean,
      default: false,
    },
    hideCancel: {
      type: Boolean,
      default: false,
    },
  },
  data() {
    return {
      isOpen: this.initIsOpen,
      okCallbackActive: false,
      icons: {
        mdiCloseThick,
      },
      ariaLabel: `dialog-${new Date().getTime()}`,
    };
  },
  methods: {
    setState(stateBool) {
      this.isOpen = stateBool;
    },
    close() {
      this.isOpen = false;
    },
  },
});
</script>

<style scoped>
.close-icon {
  position: absolute;
  top: 1rem;
  right: 1rem;
  cursor: pointer;
}
</style>
