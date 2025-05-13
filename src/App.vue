<template>
  <div
    id="container"
    class="w-full h-screen bg-sc-orange/80 bg-linear-to-r from-sc-orange to-sc-orange/80 bg-[url(assets/bg-4.jpg)] bg-center-center bg-blend-multiply bg-cover bg-no-repeat relative flex flex-col items-center justify-center px-8"
  >
    <h2 class="font-medium text-3xl mb-4 hidden">active</h2>
    <RadialProgress :progress="progress" class="">
      <h2 class="font-black text-7xl mb-4">
        {{ remainingTime }}
      </h2>
    </RadialProgress>

    <div
      class="size-12 absolute right-6 bottom-6 bg-sc-faint rounded-full p-2 cursor-pointer shadow"
    >
      <Cog6ToothIcon class="size-8 text-sc-orange" />
    </div>
  </div>
</template>

<script lang="ts" setup>
import { ref, computed } from "vue";
import { Cog6ToothIcon } from "@heroicons/vue/20/solid";
import { useCountdown } from "@vueuse/core";
import RadialProgress from "./components/RadialPRogress.vue";
// Constants
const SESSION_DURATION = 25 * 60 * 1000; // 25 minutes in ms
const BREAK_DURATION = 5 * 60 * 1000; // 5 minutes in ms
const progress = ref(70);
const remainingTime = ref(formatMilliseconds(SESSION_DURATION));
// window.setInterval(() => {
//   let timer = SESSION_DURATION;
//   while (timer != 0) {
//     remainingTime.value = formatMilliseconds(timer - 1);
//   }
//   timer -= 1;
// }, 1000);

function formatMilliseconds(ms: number): string {
  const totalSeconds = Math.floor(ms / 1000);
  const minutes = Math.floor(totalSeconds / 60);
  const seconds = totalSeconds % 60;
  return `${minutes}:${seconds.toString().padStart(2, "0")}`;
}
</script>

<style scoped></style>
