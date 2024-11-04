<template>
  <label
    :for="id"
    class="block border border-gray-200 p-3.5 rounded-lg"
    :class="{
      'border-teal-500': isSelected,
      'border-gray-200 cursor-pointer': !isSelected,
    }"
    @click="debug"
  >
    <p
      class="flex justify-between items-center"
      :class="{
        'font-bold text-neutral-800': isSelected,
        'text-neutral-600': !isSelected,
      }"
    >
      {{ label }}
      <input
        type="radio"
        :name="name"
        :id="id"
        class="hidden"
        :value="value"
        @change="$emit('update:modelValue', value)"
      />
      <span
        class="w-5 h-5 appearance-none border border-gray-200 rounded-full flex items-center justify-center"
        :class="{
          'bg-teal-500 border-teal-500': isSelected,
          'border-gray-200': !isSelected,
        }"
      >
        <img :src="checkIcon" alt="checkIcon" class="w-5 h-5" />
      </span>
    </p>
    <p class="text-sm mt-0.5">{{ priceText }}</p>
  </label>
</template>

<script setup>
import { defineProps } from 'vue';
import checkIcon from "@/assets/images/check.svg";

const props = defineProps({
  id: String,
  label: String,
  name: String,
  value: String,
  modelValue: String,
  priceText: String,
  isSelected: Boolean,
});

const debug = () => {
  console.log()
}
</script>