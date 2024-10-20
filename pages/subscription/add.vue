<script setup>
import { useNuxtApp } from "#app";
import { doc, setDoc, collection, query, getDocs, where } from "firebase/firestore";

import Service from "~/components/Service.vue";

const inputService = useState("inputService", () => "サービス名");
const inputPrice = useState("inputPrice", () => 100);

const { $db } = useNuxtApp();

const plansRef = collection($db, "plans");

const q = query(plansRef, where("isCustom", "==", false));

// ドキュメントを取得
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

    // データを追加
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

<template>
  <main class="flex">
    <section class="w-1/3 p-6 h-screen border-r border-gray-100">
      <div>
        <h3 class="text-xl font-bold mb-4">🔥人気のサブスクリプション</h3>
        <ul class="grid grid-cols-3 gap-2.5">
          <Service text="Amazon Prime" :isSelect="false" />

        </ul>
      </div>

      <div class="mt-12 max-w-96">
        <h3 class="text-xl font-bold mb-4">上記にないサブスクも登録できます！</h3>
        <div class="flex flex-col gap-2.5 mb-5 text-sm">
          <div class="flex items-center">
            <p class="block w-28 flex-shrink-0">サービス名</p>
            <input type="text" name="service" id="" v-model="inputService" class="w-full border border-gray-200 px-3 h-10 rounded-lg outline-teal-500 placeholder-gray-300" />
          </div>

          <div class="flex items-center">
            <p class="block w-28 flex-shrink-0">利用開始日</p>
            <input type="date" name="service" id="" class="w-full border border-gray-200 px-3 h-10 rounded-lg outline-teal-500 placeholder-gray-300" />
          </div>

          <div class="flex items-center">
            <p class="block w-28 flex-shrink-0">金額</p>
            <div class="flex items-center w-full gap-2">
              <select name="" id="" class="text-sm border border-gray-200 px-3 h-10 rounded-lg outline-teal-500 placeholder-gray-300">
                <option value="monthly">月額</option>
                <option value="yearly">年額</option>
              </select>
              <input type="number" v-model="inputPrice" name="service" id="" class="border border-gray-200 w-full px-3 h-10 rounded-lg outline-teal-500 placeholder-gray-300" />
              <span class="text-xs font-bold">円</span>
            </div>
          </div>
        </div>
        <button @click="addData" class="border duration-200 hover:bg-teal-50 border-teal-500 text-teal-500 w-full h-10 rounded-lg font-bold">登録する</button>
      </div>
    </section>

    <section class="w-1/3 p-6 h-screen">
      <h3 class="text-xl font-bold mb-3">プランを選択する</h3>
    </section>
  </main>
</template>
