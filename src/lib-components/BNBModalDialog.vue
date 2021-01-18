<template>
  <div class="bnb-modal-dialog always-present-wrapper">
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
          class="flex items-start justify-center min-h-screen px-4 pb-16 text-center sm:block sm:p-0"
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
            class="inline-block bg-white text-gray-700 rounded-lg text-left overflow-hidden shadow-xl transform transition-all p-4 sm:my-8 sm:align-middle sm:max-w-lg sm:w-full"
            role="dialog"
            aria-modal="true"
            :aria-labelledby="ariaLabel"
          >
            <!-- Heading -->
            <div
              class="as-h4 modal-title flex space-between text-gray-800 font-bold px-4 py-4"
            >
              {{ title }}
              <bnb-icon
                class="ml-4 close-icon text-gray-600"
                :path-info="icons.mdiCloseThick"
                @click.native="isOpen = false"
              />
            </div>

            <!-- HTML content -->
            <div class="bg-white px-4 py-0">
              <slot />
            </div>

            <!-- Footer / Action buttons -->
            <div
              v-show="hideButtons === false"
              class="bg-white px-4 py-3 sm:px-6 sm:flex sm:flex-row-reverse"
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
import { mdiCloseThick } from './mdi.js';
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
  mounted() {
    // Move to the bottom of the body to ensure it's always relative to the body, not a container element
    const el = this.$el;
    document.body.appendChild(el); // NOTE: which is more correct: the bottom of <body> or the bottom of #app?
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
.bnb-modal-dialog {
  margin: 0;
  padding: 0;
}

.close-icon {
  /* position: absolute;
  top: 1rem;
  right: 1rem; */
  cursor: pointer;
}

/* --- */
/* If TailwindCSS isn't loaded, these rules will make things look good enough
If it IS loaded, PurgeCSS should clean things up. */

.relative {
  position: relative;
}
.absolute {
  position: absolute;
}
.fixed {
  position: fixed;
}

.z-40 {
  z-index: 40;
}

.overflow-hidden {
  overflow: hidden;
}
.overflow-y-auto {
  overflow-y: auto;
}

.inset-0 {
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
}

/* Flex */

.inline {
  display: inline;
}
.inline-block {
  display: inline-block;
}
.inline-flex {
  display: inline-flex;
}

.flex {
  display: flex;
}
.flex-wrap {
  flex-wrap: wrap;
}
.flex-auto {
  flex: 1 1 auto;
}
.justify-around {
  justify-content: space-around;
}
.justify-center {
  justify-content: center;
}
.space-between {
  justify-content: space-between;
}
.items-start {
  align-items: flex-start;
}
/* .items-end {
  align-items: flex-end;
} */
.min-h-screen {
  min-height: 100vh;
}
.align-bottom {
  vertical-align: bottom;
}

/* Background */

.bg-white {
  --bg-opacity: 1;
  background-color: #fff;
  background-color: rgba(255, 255, 255, var(--bg-opacity));
}

.bg-gray-100 {
  --tw-bg-opacity: 1;
  background-color: rgba(243, 244, 246, var(--tw-bg-opacity));
}
.bg-gray-500 {
  --tw-bg-opacity: 1;
  background-color: rgba(107, 114, 128, var(--tw-bg-opacity));
}

/* Text + Font */

.text-gray-600 {
  --text-opacity: 1;
  color: #718096;
  color: rgba(113, 128, 150, var(--text-opacity));
}
.text-gray-700 {
  --text-opacity: 1;
  color: #4a5568;
  color: rgba(74, 85, 104, var(--text-opacity));
}

.text-gray-800 {
  --text-opacity: 1;
  color: #2d3748;
  color: rgba(45, 55, 72, var(--text-opacity));
}

.text-gray-900 {
  --text-opacity: 1;
  color: #1a202c;
  color: rgba(26, 32, 44, var(--text-opacity));
}

.text-left {
  text-align: left;
}

.text-center {
  text-align: center;
}

.text-75 {
  font-size: 0.75rem;
}
.text-100 {
  font-size: 1rem;
}

.font-bold {
  font-weight: 700;
}
.font-black {
  font-weight: 900;
}

.font-system {
  font-family: Helvetica, system-ui, -apple-system, Segoe UI, Roboto, Ubuntu,
    Cantarell, Noto Sans, sans-serif, BlinkMacSystemFont, 'Segoe UI',
    'Helvetica Neue', Arial, 'Noto Sans', 'Apple Color Emoji', 'Segoe UI Emoji',
    'Segoe UI Symbol', 'Noto Color Emoji';
}

/* Border */

.border {
  border-width: 1px;
}

.border-gray-400 {
  --border-opacity: 1;
  border-color: #cbd5e0;
  border-color: rgba(203, 213, 224, var(--border-opacity));
}

.shadow-sm {
  box-shadow: 0 1px 2px 0 rgba(0, 0, 0, 0.05);
}
.shadow-xl {
  box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.1),
    0 10px 10px -5px rgba(0, 0, 0, 0.04);
}

.rounded-sm {
  border-radius: 0.125rem;
}
.rounded-md {
  border-radius: 0.375rem;
}
.rounded-lg {
  border-radius: 0.5rem;
}

/* Shadow */

.shadow {
  box-shadow: 0 1px 3px 0 rgba(0, 0, 0, 0.1), 0 1px 2px 0 rgba(0, 0, 0, 0.06);
}

/* Margin and Padding */

.m-4 {
  margin: 1rem;
}

.mt-0 {
  margin-top: 0;
}
.mt-3 {
  margin-top: 0.75rem;
}
.mt-4 {
  margin-top: 1rem;
}
.mt-12 {
  margin-top: 3rem;
}

.mb-0 {
  margin-bottom: 0;
}
.mb-2 {
  margin-bottom: 0.5rem;
}
.mb-4 {
  margin-bottom: 1rem;
}
.mb-8 {
  margin-bottom: 2rem;
}

.ml-4 {
  margin-left: 1rem;
}
.ml-12 {
  margin-left: 3rem;
}

.my-0 {
  margin-top: 0;
  margin-bottom: 0;
}

.p-0 {
  padding: 0;
}
.p-3 {
  padding: 0.75rem;
}
.p-4 {
  padding: 1rem;
}

.pb-16 {
  padding-bottom: 4rem;
}

.px-0 {
  padding-left: 0;
  padding-right: 0;
}
.px-4 {
  padding-left: 1rem;
  padding-right: 1rem;
}

.py-0 {
  padding-top: 0;
  padding-bottom: 0;
}
.py-2 {
  padding-top: 0.5rem;
  padding-bottom: 0.5rem;
}
.py-3 {
  padding-top: 0.75rem;
  padding-bottom: 0.75rem;
}
.py-4 {
  padding-top: 1rem;
  padding-bottom: 1rem;
}

/* Misc */

.left-0 {
  left: 0;
}
.top-0 {
  top: 0;
}
.-top-1 {
  top: -1px;
}

.w-12 {
  width: 48px;
}
.w-24 {
  width: 96px;
}
.w-4\/5 {
  width: 80%;
}
.w-full {
  width: 100%;
}

.h-12 {
  height: 48px;
}

.opacity-0 {
  opacity: 0;
}
.opacity-75 {
  opacity: 0.75;
}
.opacity-100 {
  opacity: 1;
}

.no-underline {
  text-decoration: none;
}

.cursor-pointer {
  cursor: pointer;
}

.transition {
  transition-property: background-color, border-color, color, fill, stroke,
    opacity, box-shadow, transform;
}
.transition-opacity {
  transition-property: opacity;
}
.transition-all {
  transition-property: all;
}
.transform {
  --transform-translate-x: 0;
  --transform-translate-y: 0;
  --transform-rotate: 0;
  --transform-skew-x: 0;
  --transform-skew-y: 0;
  --transform-scale-x: 1;
  --transform-scale-y: 1;
  transform: translateX(var(--transform-translate-x))
    translateY(var(--transform-translate-y)) rotate(var(--transform-rotate))
    skewX(var(--transform-skew-x)) skewY(var(--transform-skew-y))
    scaleX(var(--transform-scale-x)) scaleY(var(--transform-scale-y));
}
.duration-200 {
  transition-duration: 200ms;
}
.duration-300 {
  transition-duration: 300ms;
}
.ease-in {
  transition-timing-function: cubic-bezier(0.4, 0, 1, 1);
}
.ease-out {
  transition-timing-function: cubic-bezier(0, 0, 0.2, 1);
}

/* SMALL */
@media (min-width: 640px) {
  .sm\:block {
    display: block;
  }
  .sm\:inline-block {
    display: inline-block;
  }

  .sm\:p-0 {
    padding: 0;
  }
  .sm\:px-6 {
    padding-left: 1.5rem;
    padding-right: 1.5rem;
  }

  .sm\:mt-0 {
    margin-top: 0;
  }
  .sm\:ml-3 {
    margin-left: 0.75rem;
  }
  .sm\:my-8 {
    margin-top: 2rem;
    margin-bottom: 2rem;
  }

  .sm\:h-screen {
    height: 100vh;
  }
  .sm\:flex {
    display: flex;
  }
  .sm\:flex-row-reverse {
    flex-direction: row-reverse;
  }

  .sm\:align-middle {
    vertical-align: middle;
  }
  .sm\:max-w-lg {
    max-width: 32rem;
  }
  .sm\:w-auto {
    width: auto;
  }
  .sm\:w-full {
    width: 100%;
  }
}

/* MEDIUM */
@media (min-width: 768px) {
  .md\:absolute {
    position: absolute;
  }
  .md\:fixed {
    position: fixed;
  }

  .md\:ml-0 {
    margin-left: 0;
  }
  .md\:mt-0 {
    margin-top: 0;
  }
  .md\:mt-32 {
    margin-top: 8rem;
  }
  .md\:inline {
    display: inline;
  }

  .md\:block {
    display: block;
  }
}
</style>
