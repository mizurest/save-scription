<template>
  <main class="flex justify-center">
    <section class="w-1/3 p-8 h-screen border-r border-gray-200">
      <div>
        <ul class="flex border-b border-gray-200 gap-6">
          <li class="py-3 border-b-2 border-black font-bold">すべて</li>
          <li class="py-3 text-gray-400 cursor-pointer">動画</li>
          <li class="py-3 text-gray-400 cursor-pointer">音楽</li>
          <li class="py-3 text-gray-400 cursor-pointer">本</li>
          <li class="py-3 text-gray-400 cursor-pointer">その他</li>
        </ul>
        <ul class="grid grid-cols-3 gap-2.5 mt-5">
          <Service :text="s.service_name" :isSelect="selectServiceId === s.id" :logo="s.icon_name" v-for="s in services" :key="s.id" @click="selectServiceId = s.id" />
        </ul>
      </div>
    </section>

    <section class="w-1/3 p-8 h-screen">
      <h3 class="text-lg font-bold mb-2">プランを選択する</h3>
      <p class="text-neutral-400 text-xs mb-2.5">選択中のサービス - Amazon Prime</p>
      <div class="flex flex-col gap-2.5 mb-8">
        <PlanRadioButton id="plan1" label="年間プラン" name="plan" value="年間プラン" v-model="selectedPlan" :isSelected="selectedPlan === '年間プラン'" priceText="￥5,900 / 年" />
        <PlanRadioButton id="plan2" label="月間プラン" name="plan" value="月間プラン" v-model="selectedPlan" :isSelected="selectedPlan === '月間プラン'" priceText="￥600 / 月" />
      </div>
      <div class="w-48 text-sm mb-3">
        <p class="block w-28 flex-shrink-0 text-neutral-600 mb-1">利用開始日</p>
        <input type="date" name="service" id="" value="2024-11-05" class="w-full border border-gray-200 px-3 h-10 rounded-lg outline-teal-500 placeholder-gray-300" />
      </div>
      <div class="text-sm mb-8 h-auto">
        <p class="flex items-center gap-1 text-neutral-600 mb-1">
          退会用URL
          <span class="text-xs bg-neutral-300 text-white rounded-full px-2 py-1">任意</span>
        </p>
        <input
          type="text"
          name="service"
          id=""
          value="https://www.amazon.co.jp/mc/pipelines/cancellation?ref_=hp_csp_mm_T1"
          class="w-full border border-gray-200 px-3 h-10 rounded-lg outline-teal-500 placeholder-gray-300"
        />
      </div>
      <button @click="addData" class="text-sm duration-200 bg-teal-500 text-white w-full h-10 rounded-lg">登録する</button>
    </section>
  </main>
</template>

<script setup>
import { useNuxtApp } from "#app";
import { ref, onMounted } from "vue";

import Service from "~/components/Service.vue";

const { $db } = useNuxtApp();
const { $supabase } = useNuxtApp();

const selectedPlan = ref("");
const services = ref([]);
const selectServiceId = ref(0);

const fetchServices = async () => {
  const { data, error } = await $supabase.from("services").select("*");

  if (error) {
    console.error("サービスの取得に失敗しました:", error);
  } else {
    services.value = data;
  }
};

onMounted(() => {
  fetchServices();
});
</script>
