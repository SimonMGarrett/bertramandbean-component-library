<template>
  <div id="app">
    <bnb-share-links />
    <div class="component-table">
      <h4>Captcha</h4>
      <bnb-captcha class="captcha-component" />
      <hr />

      <h4>Icon</h4>
      <bnb-icon color="#888" :path-info="icons.mdiTwitter" />
      <hr />

      <h4>Avatar</h4>
      <bnb-avatar color="#888" :article="article" />
      <hr />

      <h4>Info button + Modal dialog</h4>
      <bnb-info-button title="Info button and modal dialog"
        >CONTENT!</bnb-info-button
      >
      <hr />

      <h4>Link</h4>
      <bnb-link path="/pricing-page" color="#ddd">Pricing page</bnb-link>
      <hr />

      <h4>Spinner</h4>
      <bnb-spinner :show="true" color="#336688" />
      <hr />

      <h4>Tabs</h4>
      <bnb-tabs
        text-color="#333"
        underline-color="#c00"
        :tabs="tabs"
        :initial-current-tab="currentTab"
      />
    </div>
  </div>
</template>

<script>
import Vue from 'vue';
import { BNBCaptcha } from '@/entry';
import { BNBIcon } from '@/entry';
import { BNBAvatar } from '@/entry';
import { BNBInfoButton } from '@/entry';
import { BNBLink } from '@/entry';
import { BNBModalDialog } from '@/entry';
import { BNBShareLinks } from '@/entry';
import { BNBSpinner } from '@/entry';
import { BNBTabs } from '@/entry';
import { mdiTwitter } from '@/entry';

// Export
export default Vue.extend({
  name: 'ServeDev',
  components: {
    'bnb-captcha': BNBCaptcha,
    'bnb-icon': BNBIcon,
    'bnb-avatar': BNBAvatar,
    'bnb-info-button': BNBInfoButton,
    'bnb-link': BNBLink,
    'bnb-modal-dialog': BNBModalDialog,
    'bnb-share-links': BNBShareLinks,
    'bnb-spinner': BNBSpinner,
    'bnb-tabs': BNBTabs,
  },
  data() {
    return {
      // Test data to pass to components
      article: {
        createdAt: 'Sun, Jan 10 2021',
        author: {
          img: require('../src/lib-components/img/smjg-sqr.jpg'),
          name: 'Simon Garrett',
        },
      },
      tabs: [
        {
          title: 'Home',
          value: '/',
        },
        {
          title: 'Pricing long title',
          value: '/pricing',
        },
        {
          title: 'About us',
          value: '/about-us',
        },
        {
          title: 'Services',
          value: '/services',
        },
      ],
      currentTab: '/pricing',
      icons: { mdiTwitter },
    };
  },
});
</script>

<style>
h4 {
  color: #333;
}
.component-table {
  margin-left: 4rem;
  /* background: #888; */
  color: white;
  padding: 1rem;
}
.captcha-component {
  margin-bottom: 1rem;
}

/* Tailwind rule does this, we need to overcome it in all cases */
a {
  color: rgba(45, 55, 72, 1) !important;
}
</style>
