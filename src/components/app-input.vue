<script lang="ts">
export default {
  emits: ['update:modelValue'],
  props: {
    label: {
      type: String,
      default: '',
    },
    modelValue: {
      type: [Number, String],
    },
  }
};
</script>
<template>
  <div class="col-span-6 sm:col-span-2">
    <label for="first-name" class="block text-sm font-medium text-gray-700">
      {{ label }}
    </label>
    <input
      :value="modelValue"
      @input="$emit('update:modelValue', $event.target.value)"
      type="number"
      id="first-name"
      class="
        mt-1
        p-2
        focus:ring-indigo-500 focus:border-indigo-500
        block
        border
        w-full
        shadow-sm
        sm:text-sm
        border-gray-300
        rounded-md
      "
    />
  </div>
</template>
