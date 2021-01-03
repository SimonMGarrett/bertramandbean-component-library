<template>
  <div
    class="wrapper relative md:fixed flex md:block left-0 top-0 ml-2 md:ml-0 mt:0 md:mt-32 font-system"
  >
    <div
      v-for="(link, indx) in shareLinks"
      :key="indx"
      class="share-link relative w-12 h-12 bg-gray-500 text-white cursor-pointer"
      :style="`background-color: ${link.bgColor}`"
    >
      <a :href="link.url" class="text-white" style="line-height: 24px;">
        <div class="p-3">
          <bnb-icon
            :path-info="link.iconSVGPath"
            class="relative top-0 text-white"
            display="inline"
          />
        </div>
        <div
          class="share-link-text absolute left-0 top-0 w-24 h-12 text-white ml-12"
          :style="`background-color: ${link.bgColor}`"
        >
          <div class="py-3">
            {{ link.displayText }}
          </div>
        </div>
      </a>
    </div>
  </div>
</template>

<script>
import {
  mdiTwitter,
  mdiWhatsapp,
  mdiFacebook,
  mdiLinkedin,
  mdiEmail,
} from '@mdi/js/mdi.js';
import BNBIcon from './BNBIcon.vue';

export default {
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
};
</script>

<style>
.share-link > a > div.share-link-text {
  overflow: hidden;
  max-width: 0;
  font-weight: bold;
  transition: max-width 0.25s ease-out;
}
.share-link:hover > a > div {
  max-width: 84px;
}
</style>
