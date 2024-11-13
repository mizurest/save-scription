<template>
  <button
    @click="onClick"
    class="text-sm duration-200 text-white w-full h-10 rounded-lg"
    :class="{ 'bg-blue-100 cursor-not-allowed': isDisabled, 'bg-blue-800 cursor-pointer hover:bg-blue-900': !isDisabled }"
  >
    <slot></slot>
  </button>
</template>

<script setup>
import { defineProps } from "vue";

const props = defineProps({
  isDisabled: Boolean,
  onClick: Function,
});
</script>
