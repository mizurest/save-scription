<script setup>
const props = defineProps({
  text: {
    type: String,
    required: true,
    default: "",
  },
  isSelect: {
    type: Boolean,
    required: true,
    default: false,
  },
});
</script>

<template>
  <li
    class="w-full text-sm font-bold flex items-center h-12 border rounded-md px-3"
    :class="{
      'border-teal-500 text-teal-500': isSelect,
    }"
  >
    {{ text }}
  </li>
</template>
