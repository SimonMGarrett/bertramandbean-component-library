<template>
  <div
    class="relative bg-gray-100 border border-gray-400 shadow-sm rounded-sm px-4 py-4"
  >
    <!-- hidden -->
    <input
      v-model="honeypot"
      class="honeypot absolute"
      type="hidden"
      name="first_name"
    />

    <!-- test area -->
    <div class="relative test-area bg-transparent">
      <div class="test mb-4">
        <div class="inner-wrapper py-2 bg-white shadow">
          <div class="shapes-container flex justify-around mb-4">
            <bnb-icon
              class="shape circle text-gray-800"
              :class="user.shapeChosen === 'circle' ? 'checked-shape' : ''"
              :path-info="icons.mdiCheckboxBlankCircleOutline"
              @click.native="userChoosesShape('circle')"
            />
            <bnb-icon
              class="shape square text-gray-800"
              :class="user.shapeChosen === 'square' ? 'checked-shape' : ''"
              :path-info="icons.mdiSquareOutline"
              @click.native="userChoosesShape('square')"
            />
            <bnb-icon
              class="shape triangle text-gray-800"
              :class="user.shapeChosen === 'triangle' ? 'checked-shape' : ''"
              :path-info="icons.mdiTriangleOutline"
              @click.native="userChoosesShape('triangle')"
            />
          </div>

          <!-- challenge text -->
          <div class="challenge text-gray-900 text-center my-0 py-0">
            Tap or click the <span class="challenge-chosen">square</span>
          </div>
        </div>
      </div>

      <!-- Not a robot -->
      <div class="relative checkbox-area text-left">
        <div
          v-for="(checkbox, indx) in checkboxes"
          :key="indx"
          class="checkbox-wrapper relative"
          :class="
            checkboxToChoose === checkbox.num
              ? `chosen checkbox-${checkbox.num}`
              : `off-left checkbox-${checkbox.num}`
          "
        >
          <!-- eslint-disable-next-line vue/singleline-html-element-content-newline -->
          <div
            class="checkbox"
            @click="userClicksNotRobotCheckbox(checkbox.num)"
          >
            <span class="checkmark">&#10003;</span>
          </div>
          <p
            class="helper-text relative text-gray-900 cursor-pointer"
            @click="userClicksNotRobotCheckbox(checkbox.num)"
          >
            I'm not a robot
          </p>
        </div>
      </div>

      <div class="bnb-notice relative mt-4 md:absolute md:mt-0 text-center">
        <span class="text-gray-700 text-center inline" style="font-size: 12px"
          >B+B Captcha</span
        ><br class="hidden md:inline" />
        <bnb-link
          to="/privacy"
          class="text-gray-700 text-center inline no-underline"
          style="font-size: 12px"
        >
          Privacy
        </bnb-link>
        <span
          class="relative text-gray-700 text-center inline"
          style="font-size: 12px"
          >&ndash;</span
        >
        <bnb-link
          to="/terms-and-conditions"
          class="text-gray-700 text-center inline no-underline"
          style="font-size: 12px"
        >
          Terms
        </bnb-link>
      </div>
    </div>
  </div>
</template>

<script>
import Vue from 'vue';
import {
  mdiTriangleOutline,
  mdiTriangle,
  mdiCheckboxBlankCircleOutline,
  mdiCheckboxBlankCircle,
  mdiSquare,
  mdiSquareOutline,
} from '@mdi/js/mdi.js';
import BNBIcon from './BNBIcon.vue';
import BNBLink from './BNBLink.vue';
import './index.css';

export default Vue.extend({
  name: 'BNBCaptcha',
  components: {
    'bnb-icon': BNBIcon,
    'bnb-link': BNBLink,
  },
  data() {
    return {
      honeypot: '', // start empty
      shapeToChoose: 'square',
      checkboxToChoose: 1,
      checkboxes: [
        { num: 0, clicked: false },
        { num: 1, clicked: false },
      ],
      icons: {
        mdiTriangleOutline,
        mdiTriangle,
        mdiCheckboxBlankCircleOutline,
        mdiCheckboxBlankCircle,
        mdiSquare,
        mdiSquareOutline,
      },
      user: {
        shapeChosen: null,
        robotCheckboxClicked: null,
      },
    };
  },

  mounted() {
    this.setupShapeChallenge();
    this.setupCheckbox();
  },
  methods: {
    setupShapeChallenge() {
      const shapes = ['square', 'circle', 'triangle'];
      const random = Math.floor(Math.random() * shapes.length);
      const chosenShape = shapes[random];
      this.$el.querySelector('.challenge-chosen').textContent = chosenShape;
      this.shapeToChoose = chosenShape;
    },
    userChoosesShape(choice) {
      this.user.shapeChosen = choice;
    },
    setupCheckbox() {
      const boxes = [0, 1];
      const random = Math.floor(Math.random() * boxes.length);
      this.checkboxToChoose = boxes[random];
    },
    userClicksNotRobotCheckbox(number) {
      const clickedCheckbox = this.checkboxes[number];
      clickedCheckbox.clicked = !clickedCheckbox.clicked;
      const chosenCheckboxMark = this.$el.querySelector(
        `.checkbox-${number} .checkbox .checkmark`
      );
      if (clickedCheckbox.clicked) {
        chosenCheckboxMark.style.opacity = '1';
      } else {
        chosenCheckboxMark.style.opacity = '0';
      }
      this.user.robotCheckboxClicked = number;
    },
    checkValid() {
      if (this.honeypot !== '') {
        return false;
      }
      if (this.shapeToChoose !== this.user.shapeChosen) {
        return false;
      }
      if (this.checkboxToChoose !== this.user.robotCheckboxClicked) {
        return false;
      }
      // Valid
      if (
        this.$el.querySelector('.honeypot') &&
        typeof this.$el.querySelector('.honeypot').remove === 'function'
      ) {
        // Otherwise IE11 barfs
        this.$el.querySelector('.honeypot').remove(); // we don't need this now
      }
      return {
        valid: true,
        honeypot: this.honeypot,
        shape: this.user.shapeChosen,
        checkbox: this.user.robotCheckboxClicked,
      };
    },
  },
});
</script>

<style scoped>
.shape {
  width: 2rem;
  height: 2rem;
  background-color: white;
  border-radius: 0;
  cursor: pointer;
}

.checked-shape {
  --text-opacity: 1;
  color: #fff;
  color: rgba(255, 255, 255, var(--text-opacity));
  --bg-opacity: 1;
  background-color: #2f855a;
  background-color: rgba(47, 133, 90, var(--bg-opacity));
  fill: currentColor;
}

.challenge .challenge-chosen {
  text-transform: uppercase;
}

.off-left {
  left: -9000px;
  position: absolute;
}

.checkbox-area .checkbox-wrapper .checkbox {
  border: 2px solid rgba(156, 163, 175, 1);
  border-radius: 0.125rem;
  display: inline-block;
  width: 1.5rem;
  height: 1.5rem;
  font-size: 1.5rem;
  line-height: 1.25rem; /* minus 4px for border */
  text-align: center;
  background-color: #fff;
  background-color: rgba(255, 255, 255, 0);
  cursor: pointer;
}
.checkbox-area .checkbox-wrapper .checkbox .checkmark {
  display: inline-block;
  background-color: white;
  color: rgba(47, 133, 90, 1);
  font-family: Helvetica, system-ui, -apple-system, Segoe UI, Roboto, Ubuntu,
    Cantarell, Noto Sans, sans-serif, BlinkMacSystemFont, 'Segoe UI',
    'Helvetica Neue', Arial, 'Noto Sans', 'Apple Color Emoji', 'Segoe UI Emoji',
    'Segoe UI Symbol', 'Noto Color Emoji';
  width: 1.5rem;
  height: 1.5rem;
  line-height: 1.5rem;
  opacity: 0;
  transition: opacity 0.25s ease-in-out;
}

.checkbox-area .checkbox-wrapper .helper-text {
  position: absolute;
  display: inline-block;

  line-height: 12px;
  top: -8px;
  left: 40px;
}

.bnb-notice {
  bottom: 4px;
  right: 3px;
  line-height: 0.8;
}
</style>
