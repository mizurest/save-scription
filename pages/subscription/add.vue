<template>
  <main class="flex justify-center">
    <img :src="logo" alt="" class="absolute left-5 top-5 w-32" />
    <section class="w-1/3 p-8 h-screen">
      <div>
        <TabMenu />
        <ul class="grid grid-cols-3 gap-2.5 mt-5">
          <Service :text="s.service_name" :isSelect="selectServiceId === s.id" :logo="s.icon_name" v-for="s in services" :key="s.id" @click="selectServiceId = s.id" />
        </ul>
      </div>
    </section>

    <section class="w-1/3 p-8 h-screen">
      <h3 class="text-lg font-bold mb-1">プランの選択</h3>
      <p class="text-gray-400 text-xs mb-3">加入しているプランを選択してください</p>

      <div class="flex flex-col gap-3 justify-center items-center py-14 rounded-xl" v-if="selectServiceId == 0">
        <img :src="selectImg" alt="select image" class="w-40" />
        <span class="text-sm text-gray-400">サービスを選択するとプランが表示されます</span>
      </div>

      <div class="flex flex-col gap-2.5 mb-8" v-if="selectServiceId !== 0">
        <PlanRadioButton
          :id="'plan' + p.id"
          :label="p.plan_name"
          name="plan"
          :value="p.plan_name"
          v-model="selectedPlan"
          :isSelected="selectedPlan === p.plan_name"
          :priceText="`￥${p.price.toLocaleString()} / ${p.isMonthly ? '月' : '年'}`"
          v-for="p in plans"
          :key="p.id"
        />
      </div>

      <div v-if="selectServiceId !== 0">
        <div class="w-48 text-sm mb-3">
          <p class="block w-28 flex-shrink-0 text-gray-600 mb-1">利用開始日</p>
          <input type="date" name="service" id="" :value="new Date().toISOString().split('T')[0]" class="w-full border border-gray-200 px-3 h-10 rounded-lg outline-blue-800 placeholder-gray-300" />
        </div>
        <div class="text-sm mb-8 h-auto">
          <p class="flex items-center gap-1 text-gray-600 mb-1">
            退会用URL
            <span class="text-xs bg-gray-300 text-white rounded-full px-2 py-1">任意</span>
          </p>
          <input
            type="text"
            name="service"
            id=""
            value="https://www.amazon.co.jp/mc/pipelines/cancellation?ref_=hp_csp_mm_T1"
            class="w-full border border-gray-200 px-3 h-10 rounded-lg outline-blue-800 placeholder-gray-300"
          />
        </div>
        <PrimaryButton :isDisabled="false" :onClick="addData">登録する</PrimaryButton>
      </div>
    </section>
  </main>
</template>

<script setup>
import { useNuxtApp } from "#app";
import { ref, onMounted } from "vue";

import Service from "~/components/Service.vue";
import selectImg from "@/assets/images/undraw_click_here_re_y6uq.svg";
import logo from "@/assets/images/logo.svg";

const { $supabase } = useNuxtApp();

const selectedPlan = ref("");
const services = ref([]);
const plans = ref([]);
const selectServiceId = ref(0);

const fetchServices = async () => {
  const { data, error } = await $supabase.from("services").select("*");

  if (error) {
    console.error("サービスの取得に失敗しました:", error);
  } else {
    services.value = data;
  }
};

const addData = () => {};

onMounted(() => {
  fetchServices();
});

watch(selectServiceId, async (newServiceId) => {
  const { data, error } = await $supabase.from("plans").select("*").eq("service_id", newServiceId);

  if (error) {
    console.error("プラン情報の取得に失敗しました:", error);
  } else {
    plans.value = data;
  }
});
</script>
