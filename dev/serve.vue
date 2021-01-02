<script>
import Vue from 'vue';
import { BNBCaptcha } from '@/entry';
import { BNBIcon } from '@/entry';
import { BNBInfoButton } from '@/entry';
import { BNBLineItemGroups } from '@/entry';
import { BNBLink } from '@/entry';
import { BNBModalDialog } from '@/entry';
import { BNBShareLinks } from '@/entry';
import { BNBSpinner } from '@/entry';
import { BNBTabs } from '@/entry';

export default Vue.extend({
  name: 'ServeDev',
  components: {
    'bnb-captcha': BNBCaptcha,
    'bnb-icon': BNBIcon,
    'bnb-info-button': BNBInfoButton,
    'bnb-line-item-groups': BNBLineItemGroups,
    'bnb-link': BNBLink,
    'bnb-modal-dialog': BNBModalDialog,
    'bnb-share-links': BNBShareLinks,
    'bnb-spinner': BNBSpinner,
    'bnb-tabs': BNBTabs,
  },
});
</script>

<template>
  <div id="app">
    <bnb-captcha />
    <bnb-icon />
    <bnb-info-button />
    <bnb-line-item-groups />
    <bnb-link />
    <bnb-modal-dialog />
    <bnb-share-links />
    <bnb-spinner />
    <bnb-tabs />
  </div>
</template>
