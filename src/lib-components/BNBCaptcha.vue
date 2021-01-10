<template>
  <div class="bnb-captcha">
    <!-- hidden -->
    <input
      v-model="honeypot"
      class="honeypot"
      type="hidden"
      name="first_name"
    />

    <div class="shapes-test">
      <div class="shapes-container">
        <bnb-icon
          class="shape circle"
          :class="user.shapeChosen === 'circle' ? 'checked-shape' : ''"
          :path-info="icons.mdiCheckboxBlankCircleOutline"
          @click.native="userChoosesShape('circle')"
        />
        <bnb-icon
          class="shape square"
          :class="user.shapeChosen === 'square' ? 'checked-shape' : ''"
          :path-info="icons.mdiSquareOutline"
          @click.native="userChoosesShape('square')"
        />
        <bnb-icon
          class="shape triangle"
          :class="user.shapeChosen === 'triangle' ? 'checked-shape' : ''"
          :path-info="icons.mdiTriangleOutline"
          @click.native="userChoosesShape('triangle')"
        />
      </div>

      <!-- challenge text -->
      <div class="challenge-text">
        Tap or click the <span class="challenge-chosen">square</span>
      </div>
    </div>

    <!-- Not a robot -->
    <div class="not-a-robot-test">
      <div
        v-for="(checkbox, indx) in checkboxes"
        :key="indx"
        class="checkbox-wrapper"
        :class="
          checkboxToChoose === checkbox.num
            ? `chosen checkbox-${checkbox.num}`
            : `off-left checkbox-${checkbox.num}`
        "
      >
        <!-- eslint-disable-next-line vue/singleline-html-element-content-newline -->
        <div class="checkbox" @click="userClicksNotRobotCheckbox(checkbox.num)">
          <span class="checkmark">&#10003;</span>
        </div>
        <p
          class="helper-text"
          @click="userClicksNotRobotCheckbox(checkbox.num)"
        >
          I'm not a robot
        </p>
      </div>
    </div>

    <!-- B+B/Privacy/Terms -->
    <div class="bnb-notice">
      B+B Captcha
      <br class="bnb-notice__line-break" />
      <span class="bnb-notice__separator"> : </span>
      <bnb-link path="/privacy" color="#697383" text-decoration="none"
        >Privacy</bnb-link
      >
      &ndash;
      <bnb-link
        path="/terms-and-conditions"
        color="#697383"
        text-decoration="none"
        >Terms</bnb-link
      >
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
} from './mdi.js';
import BNBIcon from './BNBIcon.vue';
import BNBLink from './BNBLink.vue';

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
.bnb-captcha {
  position: relative;
  box-sizing: border-box;
  font-family: Helvetica, system-ui, -apple-system, Segoe UI, Roboto, Ubuntu,
    Cantarell, Noto Sans, sans-serif, BlinkMacSystemFont, 'Segoe UI',
    'Helvetica Neue', Arial, 'Noto Sans', 'Apple Color Emoji', 'Segoe UI Emoji',
    'Segoe UI Symbol', 'Noto Color Emoji';
  background-color: #f7fafc;
  border: 1px solid #cbd5e0;
  box-shadow: 0 1px 2px 0 rgba(0, 0, 0, 0.05);
  border-radius: 0.125rem;
  padding: 1rem;
}

.shapes-test {
  margin-bottom: 1rem;
  padding: 0.5rem 0;
  background-color: white;
  /* box-shadow: 0 2px 8px 0 rgba(0, 0, 0, 0.15); */
  border: 1px solid rgb(203, 213, 224, 0.5);
}

.shapes-test .shapes-container {
  display: flex;
  justify-content: space-around;
  margin-bottom: 1rem;
}

.shapes-test .shapes-container .shape {
  width: 2rem;
  height: 2rem;
  color: #2d3748;
  background-color: white;
  border-radius: 0;
  cursor: pointer;
}

.shapes-test .shapes-container .checked-shape {
  --text-opacity: 1;
  color: #fff;
  color: rgba(255, 255, 255, var(--text-opacity));
  --bg-opacity: 1;
  background-color: #2f855a;
  background-color: rgba(47, 133, 90, var(--bg-opacity));
  fill: currentColor;
}

.challenge-text {
  color: #2d3748;
  text-align: center;
  margin: 0;
  padding: 0;
  margin-bottom: 0.25rem;
}
.challenge-text .challenge-chosen {
  text-transform: uppercase;
}

.off-left {
  left: -9000px;
  position: absolute;
}

.not-a-robot-test {
  text-align: left;
}

.not-a-robot-test .checkbox-wrapper .checkbox {
  box-sizing: border-box;
  border: 2px solid rgba(156, 163, 175, 1);
  border-radius: 0.125rem;
  display: inline-block;
  width: 1.5rem;
  height: 1.5rem;
  font-size: 1.5rem;
  line-height: 1.25rem; /* minus 4px for border */
  text-align: center;
  background-color: #fff;
  cursor: pointer;
}
.not-a-robot-test .checkbox-wrapper .checkbox .checkmark {
  display: inline-block;
  background-color: white;
  color: rgba(47, 133, 90, 1);
  width: 1.25rem;
  height: 1.25rem;
  line-height: 1.25rem;
  opacity: 0;
  transition: opacity 0.25s ease-in-out;
}

.not-a-robot-test .checkbox-wrapper .helper-text {
  position: relative;
  top: -2px;
  left: 8px;
  display: inline-block;
  line-height: 1.25rem;
  margin: 0;
  padding: 0;
  color: #2d3748;
  cursor: pointer;
}

.bnb-notice {
  font-size: 12px;
  color: #697383;
  position: relative;
  bottom: -0.25rem;
  right: 0;
  text-align: center;
  line-height: 1.1;
}
.bnb-notice__line-break {
  display: none;
}
.bnb-notice__separator {
}
@media (min-width: 768px) {
  .bnb-notice {
    position: absolute;
    bottom: 1rem;
    right: 1rem;
    text-align: right;
  }
  .bnb-notice__line-break {
    display: inline;
  }
  .bnb-notice a {
    text-decoration: none;
  }
  .bnb-notice__separator {
    display: none;
  }
}
</style>
