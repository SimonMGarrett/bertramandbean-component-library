<script>
import Vue from 'vue';
import { BNBCaptcha } from '@/entry';
import { BNBIcon } from '@/entry';
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

<template>
  <div id="app">
    <bnb-share-links />
    <div class="ml-12 bg-gray-700 text-white p-3">
      <h4>Captcha</h4>
      <bnb-captcha />
      <hr />

      <h4>Icon</h4>
      <bnb-icon :path-info="icons.mdiTwitter" />
      <hr />

      <h4>Info button + Modal dialog</h4>
      <bnb-info-button />
      <hr />

      <h4>Link</h4>
      <bnb-link path="/pricing-page" color="#ddd">Pricing page</bnb-link>
      <hr />

      <h4>Spinner</h4>
      <bnb-spinner :show="true" />
      <hr />

      <h4>Tabs</h4>
      <bnb-tabs :tabs="tabs" :current-tab="currentTab" />
    </div>
  </div>
</template>
