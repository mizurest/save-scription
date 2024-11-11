<template>
  <li
    class="w-full text-sm flex items-center h-12 border rounded-lg px-3 text-gray-600 truncate"
    :class="{
      'font-bold border-slate-950 bg-neutral-50 text-neutral-950': isSelect,
      'cursor-pointer hover:bg-neutral-50 duration-200': !isSelect && !isLoading,
    }"
  >
    <img class="mr-1.5 h-5 w-5" :src="icons[logo]" alt="Logo" v-if="!isLoading"/>
    <span v-if="!isLoading">{{ text }}</span>

    <div class="animate-pulse rounded-full bg-gray-100 h-4 w-4" v-if="isLoading"></div>
    <div class="ml-1 animate-pulse h-2.5 w-2/3 bg-gray-100 rounded" v-if="isLoading"></div>
    
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
    required: false,
    default: "",
  },
});

const icons = ref({});
const loadIcons = async () => {
  const images = import.meta.glob("@/assets/images/*.svg");

  for (const path in images) {
    const fileName = path.split("/").pop(); // ファイル名を取得
    icons.value[fileName] = (await images[path]()).default; // 画像を非同期でインポート
  }
};

onMounted(() => {
  loadIcons();
});
</script>
