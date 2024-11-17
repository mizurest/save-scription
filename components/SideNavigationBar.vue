<template>
  <section class="flex flex-col justify-between w-80 shrink-0 h-screen px-5 pt-10 pb-5 bg-white drop-shadow-lg">
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
      <div class="relative">
        <DotsIcon class="w-9 text-gray-400 p-2 rounded-full cursor-pointer duration-200 hover:bg-gray-50" @click="onClickDots" />
        <div class="absolute bottom-9 left-0 pb-3" v-if="isDisplayLogout">
          <div class="py-1.5 z-10 bg-white w-48 rounded-lg drop-shadow-md text-sm">
            <span class="px-4 flex items-center gap-1.5 text-red-700 h-10 cursor-pointer duration-200 hover:bg-gray-50" @click="onClickLogout">
              <ExitIcon />
              ログアウト
            </span>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import Logo from "~/assets/images/logo.svg";
import DotsIcon from "~/assets/images/dots.svg";
import RocketIcon from "~/assets/images/rocket-lunch.svg";
import ChartIcon from "~/assets/images/chart.svg";
import ExitIcon from "~/assets/images/exit.svg";

const { $supabase } = useNuxtApp();
const router = useRouter();

const userEmail = ref("");
const isDisplayLogout = ref(false);

const props = defineProps({
  activeTab: {
    type: Number,
    required: true,
    default: 0,
  },
});

const getSession = async () => {
  const { data, error } = await $supabase.auth.getSession();

  if (error) {
    console.error("セッション取得エラー:", error);
    return;
  }

  userEmail.value = data.session?.user?.email;
};

const onClickDots = () => {
  isDisplayLogout.value = !isDisplayLogout.value;
};

const onClickLogout = async () => {
  const { error } = await $supabase.auth.signOut();

  if (error) {
    console.error("ログアウトエラー:", error);
    return;
  }else{
    router.push("/login");
  }
};

onMounted(async () => {
  await getSession();
});
</script>
