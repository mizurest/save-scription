<template>
  <li
    class="w-full text-sm flex items-center h-12 border rounded-lg px-3 text-gray-600"
    :class="{
      'font-bold border-slate-950 bg-neutral-50 text-neutral-950': isSelect,
    }"
  >
    <img v-if="logo" :src="logo" alt="Logo" class="mr-1.5 h-5 w-5" />
    {{ text }}
  </li>
</template>

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
  logo: {
    type: String,
    required: false,
    default: "",
  },
});
</script>