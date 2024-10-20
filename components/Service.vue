<script setup>
import NetflixLogo from "@/assets/images/netflix.svg";

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
    class="w-full text-sm flex items-center h-12 border rounded-lg px-3"
    :class="{
      'font-bold border-[#000000ee]': isSelect,
    }"
  >
  <img :src="NetflixLogo" alt="" class="mr-1.5">
    {{ text }}
  </li>
</template>
