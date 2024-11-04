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
          <Service text="Amazon Prime" :isSelect="false" :logo="AmazonPrimeLogo" />
          <Service text="Netflix" :isSelect="true" :logo="NetflixLogo" />
          <Service text="U-NEXT" :isSelect="false" :logo="UnextLogo" />
          <Service text="Hulu" :isSelect="false" :logo="HuluLogo" />
        </ul>
      </div>
    </section>

    <section class="w-1/3 p-8 h-screen">
      <h3 class="text-lg font-bold mb-2">プランを選択する</h3>
      <p class="text-neutral-400 text-xs mb-2.5">選択中のサービス - Amazon Prime</p>
      <div class="flex flex-col gap-2.5 mb-8">
        <label
          for="plan1"
          class="block border border-gray-200 p-3.5 rounded-lg"
          :class="{
            'border-teal-500': selectedPlan === '年間プラン',
            'border-gray-200 cursor-pointer': selectedPlan !== '年間プラン',
          }"
        >
          <p
            class="flex justify-between items-center"
            :class="{
              'font-bold text-neutral-800': selectedPlan === '年間プラン',
              'text-neutral-600': selectedPlan !== '年間プラン',
            }"
          >
            年間プラン
            <input type="radio" name="plan" id="plan1" class="hidden" value="年間プラン" v-model="selectedPlan" />
            <span
              class="w-5 h-5 appearance-none border border-gray-200 rounded-full flex items-center justify-center"
              :class="{
                'bg-teal-500 border-teal-500': selectedPlan === '年間プラン',
                'border-gray-200': selectedPlan !== '年間プラン',
              }"
            >
              <img :src="checkIcon" alt="checkIcon" class="w-5 h-5" />
            </span>
          </p>
          <p class="text-sm mt-0.5">￥5,900 / 年</p>
        </label>
        <label
          for="plan2"
          class="block border border-gray-200 p-3.5 rounded-lg"
          :class="{
            'border-teal-500': selectedPlan === '月間プラン',
            'border-gray-200 cursor-pointer': selectedPlan !== '月間プラン',
          }"
        >
          <p
            class="flex justify-between items-center"
            :class="{
              'font-bold text-neutral-800': selectedPlan === '月間プラン',
              'text-neutral-600': selectedPlan !== '月間プラン',
            }"
          >
            月間プラン
            <input type="radio" name="plan" id="plan2" class="hidden" value="月間プラン" v-model="selectedPlan" />
            <span
              class="w-5 h-5 appearance-none border border-gray-200 rounded-full flex items-center justify-center"
              :class="{
                'bg-teal-500 border-teal-500': selectedPlan === '月間プラン',
                'border-gray-200': selectedPlan !== '月間プラン',
              }"
            >
              <img :src="checkIcon" alt="" class="w-5 h-5" />
            </span>
          </p>
          <p class="text-sm text-neutral-600 mt-0.5">￥600 / 月</p>
        </label>
      </div>
      <div class="w-60 text-sm mb-3">
        <p class="block w-28 flex-shrink-0 text-neutral-600 mb-1">利用開始日</p>
        <input type="date" name="service" id="" value="2024-11-05" class="w-full border border-gray-200 px-3 h-10 rounded-lg outline-teal-500 placeholder-gray-300" />
      </div>
      <div class="text-sm mb-8 h-auto">
        <p class="flex gap-1 text-neutral-600 mb-1">
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
import { doc, setDoc, collection, query, getDocs, where } from "firebase/firestore";

import Service from "~/components/Service.vue";
import NetflixLogo from "@/assets/images/netflix.svg";
import AmazonPrimeLogo from "@/assets/images/amazon-prime.svg";
import UnextLogo from "@/assets/images/unext.svg";
import HuluLogo from "@/assets/images/hulu.svg";

import checkIcon from "@/assets/images/check.svg";

const inputService = useState("inputService", () => "");
const inputPrice = useState("inputPrice", () => 0);

const selectedPlan = ref("");

const { $db } = useNuxtApp();

const plansRef = collection($db, "plans");

const q = query(plansRef, where("isCustom", "==", false));

getDocs(q)
  .then((querySnapshot) => {
    querySnapshot.forEach((doc) => {
      console.log(doc.id, " => ", doc.data());
    });
  })
  .catch((error) => {
    console.log("Error getting documents: ", error);
  });

const addData = async () => {
  try {
    const docRef = doc($db, "users", "test3");

    await setDoc(docRef, {
      service: "Netflix",
      plan: "",
      price: 890,
      isCustom: true,
    });

    console.log("ドキュメントが作成されました。ID:", docRef.id);
  } catch (error) {
    console.error("ドキュメントの追加中にエラーが発生しました:", error.code);
    console.error("エラーメッセージ:", error.message);
  }
};
</script>
