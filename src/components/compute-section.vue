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
    gridRange() {
      const vm: any = this;

      let { grid, maxPrice, minPrice } = vm.formData;

      if (grid - 1 <= 0) {
        return 0;
      }

      maxPrice = new Decimal(maxPrice || 0);
      minPrice = new Decimal(minPrice || 0);

      return maxPrice.sub(minPrice).div(grid - 1);
    },
    result(): any {
      const vm: any = this;

      if (!vm.check(vm.formData)) {
        return 0;
      }

      const sale = new Decimal(vm.sale);
      const unitPreGrid = new Decimal(vm.unitPreGrid);
      let { starterPrice } = vm.formData;

      starterPrice = new Decimal(starterPrice);

      return unitPreGrid
        .mul(sale.mul(-0.001).add(vm.range.mul(1.0005)))
        .toFixed(8);
    },
  },
  methods: {
    check(formData: any) {
      const { sale, unitPreGrid, ...data } = formData;
      return Object.values(data).every((item: any) => !isNaN(parseFloat(item)));
    },
    getUnitPreGrid(formData: any) {
      let { investmentAmount, grid, sale, starterPrice } = formData;

      if (isNaN(parseFloat(investmentAmount))) {
        return 0;
      }

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

    <div>
      <span>網格間距：{{ gridRange }}</span>
    </div>

    <div>
      <span>每格利潤預測：{{ result }}</span>
    </div>
  </div>
</template>
