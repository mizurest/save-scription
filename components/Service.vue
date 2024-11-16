<template>
  <li
    class="text-sm flex items-center h-12 border rounded-lg px-3 text-gray-600 truncate"
    :class="{
      'font-bold border-slate-950 bg-neutral-50 text-neutral-950': isSelect,
      'cursor-pointer hover:bg-neutral-50 duration-200': !isSelect && !isLoading,
    }"
  >
    <img :src="icons[logo]" alt="サービスのロゴ" class="w-5 h-5 mr-2" v-if="!isLoading" />

    <span v-if="!isLoading" class="flex-grow truncate overflow-hidden whitespace-nowrap">
      {{ text }}
    </span>

    <div class="animate-pulse rounded-full bg-gray-100 h-4 w-4" v-if="isLoading"></div>
    <div class="animate-pulse ml-1 h-2.5 w-2/3 bg-gray-100 rounded" v-if="isLoading"></div>
  </li>
</template>

<script setup>
import { ref, onMounted, watch } from "vue";

const props = defineProps({
  text: {
    type: String,
    required: true,
    default: "",
  },
  isSelect: {
    type: Boolean,
    required: false,
    default: false,
  },
  isLoading: {
    type: Boolean,
    required: true,
    default: false,
  },
  logo: {
    type: String,
    required: true,
    default: "",
  },
});

const icons = ref({});
const loadIcons = async () => {
  const images = import.meta.glob("@/assets/images/service-logo/*.png");

  for (const path in images) {
    const fileName = path.split("/").pop();
    icons.value[fileName] = (await images[path]()).default;
  }
};

onMounted(async () => {
  await loadIcons();
});
</script>
