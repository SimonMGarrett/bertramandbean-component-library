<template>
  <div class="bnb-share-links">
    <div
      v-for="(link, indx) in shareLinks"
      :key="indx"
      class="share-link"
      :style="`background-color: ${link.bgColor}`"
    >
      <a :href="link.url">
        <div class="share-link-icon">
          <bnb-icon :path-info="link.iconSVGPath" display="inline" />
        </div>
        <div
          class="share-link-text"
          :style="`background-color: ${link.bgColor}`"
        >
          {{ link.displayText }}
        </div>
      </a>
    </div>
  </div>
</template>

<script>
import Vue from 'vue';
import {
  mdiTwitter,
  mdiWhatsapp,
  mdiFacebook,
  mdiLinkedin,
  mdiEmail,
} from './mdi.js';
import BNBIcon from './BNBIcon.vue';

export default Vue.extend({
  name: 'BNBShareLinks',
  components: {
    'bnb-icon': BNBIcon,
  },
  props: {
    pageToShare: {
      type: String,
      default: '/',
    },
  },
  data() {
    return {
      shareLinks: [],
      icons: {
        mdiTwitter,
        mdiWhatsapp,
        mdiFacebook,
        mdiLinkedin,
        mdiEmail,
      },
    };
  },
  mounted() {
    this.setupShareLinks();
  },
  methods: {
    setupShareLinks() {
      // const context = this.$nuxt.context;
      // const host = context.req
      //   ? context.req.headers.host
      //   : window.location.origin;
      // const pageToShare = `${host}${encodeURIComponent(this.$route.path)}`;
      const formattedBody = `TODO:\n- Change the To: address\n- Check the Subject line\n- Delete this TODO!\n----\n\nHi!\n\nI thought you might like this:\n\n${this.pageToShare}`;
      const mailToLink = `mailto:friend@something.com?subject=A webpage I thought you might like to see&body=${encodeURIComponent(
        formattedBody
      )}`;

      this.shareLinks = {
        twitter: {
          displayText: 'Twitter',
          bgColor: '#1da1f2',
          iconSVGPath: this.icons.mdiTwitter,
          url: `https://twitter.com/intent/tweet?url=${this.pageToShare}`,
        },
        whatsapp: {
          displayText: 'WhatApp',
          bgColor: '#049688',
          iconSVGPath: this.icons.mdiWhatsapp,
          // of the form -- url: `https://api.whatsapp.com/send?text=http%3A%2F%2Flocalhost%3A3030%2Fblog%2Fwhat-is-branding`
          url: `https://api.whatsapp.com/send?text=${this.pageToShare}`,
        },
        fb: {
          displayText: 'Facebook',
          bgColor: '#0a82ed',
          iconSVGPath: this.icons.mdiFacebook,
          url: `https://www.facebook.com/sharer/sharer.php?u=${this.pageToShare}`,
        },
        linkedIn: {
          displayText: 'LinkedIn',
          bgColor: '#0a66c2',
          iconSVGPath: this.icons.mdiLinkedin,
          url: `https://www.linkedin.com/shareArticle?url=${this.pageToShare}`,
        },
        email: {
          displayText: 'Email',
          bgColor: '#d43035',
          iconSVGPath: this.icons.mdiEmail,
          url: mailToLink,
        },
      };
    },
  },
});
</script>

<style scoped>
.bnb-share-links {
  position: relative;
  left: 0;
  top: 0;
  display: flex;
  z-index: 29;

  margin-left: 0.5rem;
  margin-bottom: 0.5rem;
  margin-top: 0;
  font-family: Helvetica, system-ui, -apple-system, Segoe UI, Roboto, Ubuntu,
    Cantarell, Noto Sans, sans-serif, BlinkMacSystemFont, 'Segoe UI',
    'Helvetica Neue', Arial, 'Noto Sans', 'Apple Color Emoji', 'Segoe UI Emoji',
    'Segoe UI Symbol', 'Noto Color Emoji';
}
@media (min-width: 768px) {
  .bnb-share-links {
    position: fixed;
    display: block;
    margin-left: 0;
    margin-top: 8rem;
  }
}

.bnb-share-links .share-link {
  position: relative;
  width: 3rem;
  height: 3rem;
  color: white;
  background-color: rgba(107, 114, 128, 1);
  cursor: pointer;
}

.bnb-share-links .share-link a {
  color: white;
  line-height: 24px;
}
.bnb-share-links .share-link > a > div.share-link-icon {
  padding: 0.75rem;
}
.bnb-share-links .share-link > a > div.share-link-text {
  overflow: hidden;
  max-width: 0;
  font-weight: bold;
  transition: max-width 0.25s ease-out;
  position: absolute;
  left: 0;
  top: 0;
  width: 6rem;
  height: 3rem;
  line-height: 3rem;
  color: white;
  margin-left: 3rem;
  padding: 0;
}
.bnb-share-links .share-link:hover > a > div {
  max-width: 84px;
}
</style>
