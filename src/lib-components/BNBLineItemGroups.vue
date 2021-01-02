<template>
  <div class="wrapper">
    <div
      v-for="(lineItemCollection, collectionIndx) in lineItemsToShow"
      :key="lineItemCollection.id"
      class="text-left mt-4"
    >
      <div class="font-bold text-100 mb-0 p-0">
        {{ lineItemCollection.collectionDisplayText }}:
      </div>

      <div class="block-container flex flex-wrap justify-around">
        <div
          v-for="(lineItemElem, itemIndx) in lineItemCollection.collection"
          :key="lineItemElem.id"
          class="flex-auto line-item-group"
          :class="
            lineItems[collectionIndx + start].collection[itemIndx].chosen
              ? 'checked'
              : ''
          "
        >
          <input
            :id="'line-item-' + lineItemElem.id"
            class="line-item-group__checkbox"
            type="checkbox"
            :label="lineItemElem.displayText"
            :name="`line-item-${lineItemElem.id}-${lineItemElem.name}`"
            :value="`checkbox_vmodel_${collectionIndx + start}_${itemIndx}`"
            :checked="
              lineItems[collectionIndx + start].collection[itemIndx].chosen
            "
            @change="toggleCheckbox($event, collectionIndx + start, itemIndx)"
          />

          <label class="px-0" :for="'line-item-' + lineItemElem.id">
            <div>
              <strong>{{
                lineItems[collectionIndx + start].collection[itemIndx]
                  .displayText
              }}</strong>
            </div>
            <div v-if="lineItemCollection.collectionName !== 'consulting'">
              @ &pound;{{ lineItemElem.cost | numberFormat }} + &pound;{{
                lineItemElem.monthlyCost | numberFormat
              }}/mth
            </div>
            <div v-else>10% of value increase</div>
          </label>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Vue from 'vue';

export default {
  name: 'BNBLineItemGroups',
  props: {
    start: {
      type: Number,
      default: 0,
    },
    end: {
      type: Number,
      default: 1,
    },
    lineItems: {
      type: Array,
      default: () => {
        return [];
      },
    },
  },
  computed: {
    // lineItems() {
    //   return this.$store.getters['store/lineItems'];
    // },
    lineItemsToShow() {
      return this.lineItems.slice(this.start, this.end);
    },
  },

  created() {
    Vue.filter('numberFormat', this.numberFormat); // create the filter we'll need from the method below
  },

  methods: {
    toggleCheckbox(event, collectionIndx, itemIndx) {
      this.$store.dispatch('store/toggleChosenState', {
        collectionIndx,
        itemIndx,
      });

      const parent = event.target.closest('div');
      const input = parent.querySelector('input');
      const isChecked = input.checked;
      if (isChecked) {
        parent.classList.add('checked');
      } else {
        parent.classList.remove('checked');
      }
    },

    numberFormat(number, decimals, decPoint, thousandsSep) {
      // Strip all characters but numerical ones.
      number = (number + '').replace(/[^0-9+\-Ee.]/g, '');
      const n = !isFinite(+number) ? 0 : +number;
      const precision = !isFinite(+decimals) ? 0 : Math.abs(decimals);
      const sep = typeof thousandsSep === 'undefined' ? ',' : thousandsSep;
      const dec = typeof decPoint === 'undefined' ? '.' : decPoint;
      const toFixedFix = function(n, precision) {
        const k = 10 ** precision;
        return '' + Math.round(n * k) / k;
      };
      let s = [];

      // FIX - for IE parseFloat(0.55).toFixed(0) = 0;
      s = (precision ? toFixedFix(n, precision) : '' + Math.round(n)).split(
        '.'
      );
      if (s[0].length > 3) {
        s[0] = s[0].replace(/\B(?=(?:\d{3})+(?!\d))/g, sep);
      }
      if ((s[1] || '').length < precision) {
        s[1] = s[1] || '';
        s[1] += new Array(precision - s[1].length + 1).join('0');
      }
      return s.join(dec);
    },
  },
};
</script>

<style lang="scss" scoped>
.line-item-group {
  @apply relative block leading-7 text-75 bg-gray-100 text-gray-800 border border-gray-400 rounded mr-2 p-1 pl-6 mb-1;
  // max-width: 45%;
  line-height: 1.3;

  &__checkbox {
    @apply absolute top-0 left-0 ml-1;
    margin-top: 11px;
  }
}
.line-item-group.checked {
  @apply bg-bbpink-500 text-white;

  & label {
    @apply text-white;
  }
}
</style>
