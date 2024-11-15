<template>
  <label
    :for="id"
    class="block border p-3.5 rounded-lg"
    :class="{
      'border-blue-800 text-neutral-800': isSelected,
      'border-gray-200 cursor-pointer hover:bg-neutral-50 duration-200 text-gray-600': !isSelected && !isLoading,
    }"
  >
    <div
      class="flex justify-between items-center"
      :class="{
        'font-bold': isSelected,
        '': !isSelected,
      }"
    >
      <p v-if="!isLoading">{{ label }}</p>
      <div class="ml-1 animate-pulse h-3 w-1/2 my-1.5 bg-gray-100 rounded" v-if="isLoading"></div>

      <input type="radio" :name="name" :id="id" class="hidden" :value="value" @change="$emit('update:modelValue', value)" />
      <span
        class="w-5 h-5 appearance-none border border-gray-200 rounded-full flex items-center justify-center"
        :class="{
          'bg-blue-800 border-blue-800': isSelected,
          'border-gray-200': !isSelected,
        }"
      >
        <CheckIcon class="w-5 h-5" />
      </span>
    </div>

    <p class="text-sm mt-0.5" v-if="!isLoading">{{ priceText }}</p>
    <div class="ml-1 animate-pulse h-2.5 w-1/4 my-1.5 bg-gray-100 rounded" v-if="isLoading"></div>
  </label>
</template>

<script setup>
import { defineProps } from "vue";
import CheckIcon from "~/assets/images/check.svg";

const props = defineProps({
  id: String,
  label: String,
  name: String,
  value: String,
  modelValue: String,
  priceText: String,
  isSelected: Boolean,
  isLoading: Boolean,
});
</script>
