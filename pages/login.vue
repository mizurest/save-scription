<script setup>
import logo from "@/assets/images/logo.svg";
import googleIcon from "@/assets/images/google.svg";
</script>

<template>
  <section class="flex flex-col justify-center items-center h-svh">
    <img :src="logo" alt="Logo" class="mb-9" />
    <form action="" class="flex flex-col p-5 lg:p-0 w-full lg:w-1/4">
      <div class="flex flex-col pb-14 gap-5">
        <div class="flex flex-col gap-4">
          <input type="email" name="email" id="" class="border border-gray-200 px-4 h-12 rounded-xl outline-blue-700 placeholder-gray-300" placeholder="Eメールアドレス" />
          <input type="password" name="password" id="" class="border border-gray-200 px-4 h-12 rounded-xl outline-blue-700 placeholder-gray-300" placeholder="パスワード" />
        </div>
        <button class="flex items-center bg-blue-700 hover:bg-blue-800 duration-200 text-white h-12 rounded-xl">
          <span class="block w-full pl-4">ログイン</span>
          <span class="block w-4 mr-4">→</span>
        </button>
      </div>

      <button class="flex justify-center items-center gap-1.5 border border-gray-300 bg-white hover:bg-gray-50 duration-200 h-12 rounded-xl">
        <img :src="googleIcon" alt="Google Icon" />Googleアカウントでログイン
      </button>
    </form>
  </section>
</template>
