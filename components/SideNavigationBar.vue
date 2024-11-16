<template>
  <section class="flex flex-col justify-between w-96 h-screen px-5 pt-10 pb-5 bg-white drop-shadow-lg">
    <div>
      <Logo class="w-36" />
      <div class="flex flex-col gap-1.5 mt-10 text-sm">
        <NuxtLink
          to="/subscription"
          class="px-4 w-full h-12 rounded-lg flex gap-1.5 items-center cursor-pointer"
          :class="{
            'bg-blue-50 text-blue-800 font-bold': activeTab === 0,
            'hover:bg-gray-50 duration-200': activeTab !== 0,
          }"
        >
          <RocketIcon />
          サブスクの管理
        </NuxtLink>
        <NuxtLink
          to="/chart"
          class="px-4 w-full h-12 rounded-lg flex gap-1.5 items-center cursor-pointer"
          :class="{
            'bg-blue-50 text-blue-800 font-bold': activeTab === 1,
            'hover:bg-gray-50 duration-200': activeTab !== 1,
          }"
        >
          <ChartIcon />
          分析
        </NuxtLink>
      </div>
    </div>
    <div class="w-full rounded-lg flex items-center justify-between">
      <div class="flex items-center gap-2">
        <div class="w-8 h-8 bg-blue-500 rounded-full border-gray-200"></div>
        <div>
          <p class="text-sm text-gray-700">ログイン中</p>
          <p class="text-xs text-gray-400">{{ userEmail }}</p>
        </div>
      </div>
      <DotsIcon class="w-9 text-gray-400 p-2 rounded-full cursor-pointer duration-200 hover:bg-gray-50" />
    </div>
  </section>
</template>

<script setup>
import Logo from "~/assets/images/logo.svg";
import DotsIcon from "~/assets/images/dots.svg";
import RocketIcon from "~/assets/images/rocket-lunch.svg";
import ChartIcon from "~/assets/images/chart.svg";

const props = defineProps({
  activeTab: {
    type: Number,
    required: true,
    default: 0,
  },
  userEmail: {
    type: String,
    required: true,
    default: "",
  },
});
</script>
