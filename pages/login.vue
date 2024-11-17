<template>
  <section class="flex flex-col justify-center items-center h-svh p-5 md:p-0">
    <div class="w-full md:w-[480px] flex flex-col items-center">
      <Logo class="w-48 mb-9" />
      <div v-if="isError" class="p-4 mb-6 text-sm text-red-700 bg-red-50 rounded-xl w-full">
        <p>メールアドレスまたはパスワードが正しくありません</p>
      </div>
      <form @submit.prevent="loginWithEmail" class="flex flex-col w-full">
        <div class="flex flex-col pb-14 gap-5">
          <div class="flex flex-col gap-4">
            <input type="email" name="email" id="email" v-model="email" class="border border-gray-200 px-4 h-12 rounded-xl outline-blue-700 placeholder-gray-300" placeholder="Eメールアドレス" />
            <input
              type="password"
              name="password"
              id="password"
              v-model="password"
              class="border border-gray-200 px-4 h-12 rounded-xl outline-blue-700 placeholder-gray-300"
              placeholder="パスワード"
            />
          </div>
          <button class="flex items-center bg-blue-700 hover:bg-blue-800 duration-200 text-white h-12 rounded-xl">
            <span class="block w-full pl-4">ログイン</span>
            <span class="block w-4 mr-4">→</span>
          </button>
        </div>
      </form>

      <button class="w-full flex justify-center items-center gap-1.5 border border-gray-300 bg-white hover:bg-gray-50 duration-200 h-12 rounded-xl">
        <GoogleIcon />
        Googleアカウントでログイン
      </button>
    </div>
  </section>
</template>

<script setup>
import { useHead } from "nuxt/app";

import Logo from "~/assets/images/logo.svg";
import GoogleIcon from "~/assets/images/google.svg";

const { $supabase } = useNuxtApp();
const router = useRouter();

const email = defineModel("email");
const password = defineModel("password");

const isLoadingLogin = ref(false);
const isError = ref(false);
const isErrorEmail = ref(false);
const isErrorPassword = ref(false);

useHead({
  title: "ログイン - サブカン サブスクを管理するサービス",
  meta: [
    { name: "description", content: "あなたは自分が加入しているサブスクを全て把握していますか？サブカンでは加入しているサブスクを可視化することで無駄な支出を抑えることができます。" },
    { property: "og:title", content: "ログイン - サブカン サブスクを管理するサービス" },
    { property: "og:description", content: "あなたは自分が加入しているサブスクを全て把握していますか？サブカンでは加入しているサブスクを可視化することで無駄な支出を抑えることができます。" },
  ],
});

const loginWithEmail = async () => {
  isLoadingLogin.value = true;
  const { data, error } = await $supabase.auth.signInWithPassword({
    email: email.value,
    password: password.value,
  });

  if (error) {
    console.log("ログインに失敗しました", error);

    isLoadingLogin.value = false;
    isError.value = true;
  } else {
    console.log("ログインに成功しました", data);

    isLoadingLogin.value = false;

    // キャッシュにログイン情報を保持

    // ログインに成功したら画面遷移
    router.push("/subscription");
  }
};
</script>
