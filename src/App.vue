<script lang="ts">
import Cookies from 'js-cookie';
import { AppInput, AppTitle, ComputeSection } from './components';

export default {
  components: {
    AppInput,
    AppTitle,
    ComputeSection,
  },
  data() {
    return {
      formData: {
        investmentAmount: '',
        maxPrice: '',
        minPrice: '',
        starterPrice: '',
        grid: '',
        unitPreGrid: '',
        sale: '',
      },
    };
  },
  beforeCreate() {
    const data = Cookies.get('data');

    if (data) {
      (this as any).formDat = data;
    }
  },
  beforeDestroy() {
    Cookies.set('data', (this as any).formData);
  },
};
</script>

<template>
  <div class="container mx-auto">
    <app-title />

    <div class="grid grid-cols-6 gap-6 p-5 shadow">
      <app-input label="投資額" v-model="formData.investmentAmount" />
      <app-input label="區間上限價格" v-model="formData.maxPrice" />
      <app-input label="區間下限價格" v-model="formData.minPrice" />
      <app-input label="開單時價格" v-model="formData.starterPrice" />
      <app-input label="網格數量" v-model="formData.grid" />
      <app-input label="單網格買賣數量" v-model="formData.unitPreGrid" />
      <app-input label="賣出價格" v-model="formData.sale" />
    </div>
    <compute-section :form-data="formData" />
  </div>
</template>

<style lang="scss"></style>
