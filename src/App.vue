<template>
  <div
    id="container"
    class="w-full h-screen bg-sc-orange/80 bg-linear-to-r from-sc-orange to-sc-orange/80 bg-[url(assets/bg-4.jpg)] bg-center-center bg-blend-multiply bg-cover bg-no-repeat relative flex flex-col items-center justify-center px-8"
  >
    <h2 class="font-medium text-3xl mb-4 hidden">active</h2>
    <Countdown
      :showDays="false"
      class="font-medium text-9xl"
      :deadlineDate="new Date(date)"
      countdownSize="3.75rem"
      labelSize="1.8rem"
      labelColor="var(--sc-faint)"
    />

    <div class="size-12 absolute right-6 bottom-6 bg-sc-orange rounded-full p-2 cursor-pointer shadow">
      <Cog6ToothIcon class="size-8 text-sc-faint" />
    </div>
  </div>
</template>

<script lang="ts" setup>
import {
  isPermissionGranted,
  requestPermission,
  sendNotification,
} from "@tauri-apps/plugin-notification";
import { onMounted, ref } from "vue";
import { Countdown } from "vue3-flip-countdown";
import { Cog6ToothIcon } from "@heroicons/vue/20/solid";
const percentage = ref(5);

const date = ref(new Date().getTime() + 25 * 60 * 1000);
onMounted(async () => {
  window.setInterval(() => {
    percentage.value += 0.1;
  }, 1000);
  let permissionGranted = await isPermissionGranted();
  if (!permissionGranted) {
    const permission = await requestPermission();
    permissionGranted = permission === "granted";
  }
  if (permissionGranted) {
    sendNotification({ title: "Tauri", body: "Tauri is awesome!" });
  }
});
</script>
<style scoped></style>
