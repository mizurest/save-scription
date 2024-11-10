<template>
  <li
    class="w-full text-sm flex items-center h-12 border rounded-lg px-3 text-gray-600"
    :class="{
      'font-bold border-slate-950 bg-neutral-50 text-neutral-950': isSelect,
      'cursor-pointer hover:bg-neutral-50 duration-200': !isSelect,
    }"
  >
    <img class="mr-1.5 h-5 w-5" :src="icons[logo]" alt="Logo" />
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

const icons = ref({});
const loadIcons = async () => {
  const images = import.meta.glob('@/assets/images/*.svg');

  for (const path in images) {
    const fileName = path.split('/').pop(); // ファイル名を取得（例: amazon-prime.svg）
    icons.value[fileName] = (await images[path]()).default; // 画像を非同期でインポート
  }
};

onMounted(() => {

  loadIcons()
});
</script>