<template>
  <ul class="flex border-b border-gray-200 gap-6 h-12">
    <li
      class="py-3"
      :class="{ 'border-b-2 border-black font-bold': i === selectedTabId, 'text-gray-400 cursor-pointer': i !== selectedTabId }"
      v-for="(item, i) in tabNames"
      @click="emit('update:selectedTabId', i);"
      :key="i"
    >
      {{ tabNames[i].category_name }}
    </li>
  </ul>
</template>

<script setup>
const props = defineProps({
  tabNames: {
    type: Array,
    required: true,
    default: [],
  },
  tabIds: {
    type: Array,
    required: true,
    default: [],
  },
  selectedTabId: {
    type: Number,
    required: true,
    default: 0,
  },
});

const emit = defineEmits(['update:selectedTabId']);
</script>
