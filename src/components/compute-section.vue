<script lang="ts">
import Decimal from 'decimal.js';

export default {
  props: {
    formData: {
      type: Object,
      required: true,
    },
  },
  computed: {
    isUnitPreGridSet() {
      return !!(this as any).formData.unitPreGrid;
    },
    isSaleSet() {
      return !!(this as any).formData.sale;
    },
    sale() {
      const vm: any = this;
      return vm.formData.sale || vm.formData.starterPrice;
    },
    unitPreGrid() {
      const vm: any = this;
      return vm.formData.unitPreGrid || vm.getUnitPreGrid(vm.formData);
    },
    result(): any {
      const vm: any = this;

      if (!vm.check(vm.formData)) {
        return 0;
      }

      const sale = new Decimal(vm.sale);
      const unitPreGrid = new Decimal(vm.unitPreGrid);
      let { grid, maxPrice, minPrice, starterPrice } = vm.formData;

      maxPrice = new Decimal(maxPrice);
      minPrice = new Decimal(minPrice);
      starterPrice = new Decimal(starterPrice);

      const range = maxPrice.sub(minPrice).div(grid - 1);

      return unitPreGrid.mul(sale.mul(-0.001).add(range.mul(1.0005))).toFixed(8);
    },
  },
  methods: {
    check(formData: any) {
      const { sale, unitPreGrid, ...data } = formData;
      return Object.values(data).every((item: any) => !isNaN(parseFloat(item)));
    },
    getUnitPreGrid(formData: any) {
      let { investmentAmount, grid, sale, starterPrice } = formData;

      investmentAmount = new Decimal(investmentAmount);
      sale = new Decimal(sale || starterPrice);

      return investmentAmount.div(grid - 1).div(sale);
    },
  },
};
</script>

<template>
  <div class="m-4">
    <div v-if="!isUnitPreGridSet">單網格買賣數量預測：{{ unitPreGrid }}</div>
    <div v-if="!isSaleSet">賣出價格：{{ sale }}</div>
    <span>每格利潤預測：{{result }}</span>
  </div>
</template>

<!-- { "investmentAmount": "800", "maxPrice": "45", "minPrice": "32", "starterPrice": "38.8212", "grid": "40", "unitPreGrid": "0.54" } -->
<!-- { "investmentAmount": "300", "maxPrice": "0.8", "minPrice": "0.4", "starterPrice": "0.56", "grid": "150", "unitPreGrid": "3.7", "sale": "0.55033" } 0.00790163, 0.00788473 -->
