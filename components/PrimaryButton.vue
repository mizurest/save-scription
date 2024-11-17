<template>
  <button
    @click="onClick"
    class="flex justify-center items-center duration-200 text-white h-12 rounded-xl w-full"
    :class="{ 'bg-blue-100 cursor-not-allowed': isDisabled, 'bg-blue-700 cursor-pointer hover:bg-blue-800': !isDisabled }"
  >
    <span class="flex justify-center items-center w-full pl-4">
      <LoadingCircleIcon class="animate-spin -ml-1 mr-3 h-5 w-5" v-if="isLoading" />
      <slot v-if="!isLoading"></slot>
    </span>
    <span class="block w-4 mr-4">→</span>
  </button>
</template>

<script setup>
import { defineProps } from "vue";

import LoadingCircleIcon from "~/assets/images/loading-circle.svg";

const props = defineProps({
  isDisabled: Boolean,
  isLoading: Boolean,
  onClick: Function,
});
</script>
