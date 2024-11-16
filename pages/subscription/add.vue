<template>
  <main class="flex justify-center w-full">
    <SideNavigationBar :activeTab="0" userEmail="mizurest@gmail.com"/>

    <section class="flex gap-24 w-full px-24 py-8">
      <div class="w-1/2">
        <div>
          <TabMenu :tabs="tabs" v-model:selectedTabId="selectedTabId" />
          <ul class="grid grid-cols-2 2xl:grid-cols-3 gap-2.5 mt-5">
            <Service
              v-for="s in selectedServices"
              :text="s.service_name"
              :isLoading="false"
              :isSelect="selectServiceId === s.id"
              :logo="s.icon_name"
              :key="s.id"
              @click="selectServiceId = s.id"
              v-if="!isLoadingServices"
            />
            <Service text="" :isLoading="true" :isSelected="false" v-if="isLoadingServices" v-for="n in 6" :key="n" logo="" />
          </ul>
        </div>
      </div>

      <div class="w-1/2">
        <h3 class="text-lg font-bold mb-1">プランの選択</h3>
        <p class="text-gray-400 text-xs mb-3">加入しているプランを選択してください</p>

        <div class="flex flex-col gap-5 justify-center items-center py-14 rounded-2xl bg-slate-50 mt-8" v-if="selectServiceId == 0">
          <SelectIllust class="w-40" />
          <span class="text-sm text-slate-400">サービスを選択するとプランが表示されます</span>
        </div>

        <div class="flex flex-col gap-2.5 mb-8" v-if="selectServiceId !== 0">
          <PlanRadioButton
            :id="'plan' + p.id"
            :label="p.plan_name"
            name="plan"
            :value="p.plan_name"
            v-model="selectedPlan"
            :isSelected="selectedPlan === p.plan_name"
            :isLoading="false"
            :priceText="`￥${p.price.toLocaleString()} / ${p.isMonthly ? '月' : '年'}`"
            v-for="p in plans"
            v-if="!isLoadingPlans"
            :key="p.id"
          />
          <PlanRadioButton id="" label="" name="plan" value="" :isLoading="true" v-for="n in 2" :key="n" v-if="isLoadingPlans" />
        </div>

        <div v-if="selectServiceId !== 0">
          <div class="w-48 text-sm mb-3">
            <p class="block w-28 flex-shrink-0 text-gray-500 mb-1">利用開始日</p>
            <input type="date" name="service" id="" :value="new Date().toISOString().split('T')[0]" class="w-full border border-gray-200 px-3 h-10 rounded-lg outline-blue-800 placeholder-gray-300" />
          </div>
          <div class="text-sm mb-8 h-auto">
            <p class="flex items-center gap-1 text-gray-500 mb-1">
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
      </div>
    </section>
  </main>
</template>

<script setup>
import { useNuxtApp } from "#app";
import { ref, onMounted } from "vue";

import Service from "~/components/Service.vue";
import SelectIllust from "~/assets/images/undraw_click_here_re_y6uq.svg";

const { $supabase } = useNuxtApp();

const selectedPlan = ref("");
const services = ref([]);
const selectedServices = ref([]);
const plans = ref([]);
const tabs = ref([]);
const selectedTabId = ref(0);
const selectServiceId = ref(0);
const isLoadingServices = ref(true);
const isLoadingPlans = ref(true);
const serviceCategories = ref([]); // カテゴリーごとのサービスID

const fetchCategories = async () => {
  const { data, error } = await $supabase.from("categories").select("*");

  if (error) {
    console.error("カテゴリの取得に失敗しました:", error);
  } else {
    tabs.value = data.map((item) => ({
      id: item.id,
      category_name: item.category_name,
    }));
    tabs.value.unshift({
      id: 0, // DB上にはないがすべてのidは0
      category_name: "すべて",
    });
  }
};

const fetchServiceCategories = async () => {
  const { data, error } = await $supabase.from("service_categories").select("*");

  if (error) {
    console.error("サービスカテゴリの取得に失敗しました:", error);
  } else {
    const result = data.reduce((acc, item) => {
      const { service_id, category_id } = item;

      // service_id をキーにしたプロパティがなければ初期化
      if (!acc[category_id]) {
        acc[category_id] = [];
      }

      // category_id が重複しない場合にのみ追加
      if (!acc[category_id].includes(service_id)) {
        acc[category_id].push(service_id);
      }

      return acc;
    }, {});

    serviceCategories.value = result;
  }
};

const fetchServices = async () => {
  const { data, error } = await $supabase.from("services").select("*");

  if (error) {
    console.error("サービスの取得に失敗しました:", error);
  } else {
    services.value = data;
    if (selectedTabId.value === 0) {
      selectedServices.value = services.value;
    }
  }
  isLoadingServices.value = false;
};

watch(selectServiceId, async (newServiceId) => {
  isLoadingPlans.value = true;
  const { data, error } = await $supabase.from("plans").select("*").eq("service_id", newServiceId);

  if (error) {
    console.error("プラン情報の取得に失敗しました:", error);
  } else {
    plans.value = data;
    isLoadingPlans.value = false;
  }
});

watch(selectedTabId, async (newSelectedTabId) => {
  selectedServices.value = []; // selectedTabIdが変更されたら初期化する

  if (newSelectedTabId === 0) {
    selectedServices.value = services.value;
  } else {
    serviceCategories.value[newSelectedTabId].map((item, i) => {
      const getServiceById = (id) => services.value.find((service) => service.id === id);

      selectedServices.value.push(getServiceById(item));
    });
  }

  console.log(selectedServices.value);
});

onMounted(async () => {
  await fetchServices();
  await fetchCategories();
  await fetchServiceCategories();
});

// (WIP)
const addData = () => {};
</script>
