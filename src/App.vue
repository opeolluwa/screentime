<template>
  <vue-countdown
    :time="2 * 24 * 60 * 60 * 1000"
    v-slot="{ days, hours, minutes, seconds }"
  >
    Time Remaining：{{ days }} days, {{ hours }} hours, {{ minutes }} minutes,
    {{ seconds }} seconds.
  </vue-countdown>

  <CircleProgressBar :value="7" :max="10" />
</template>

<script lang="ts" setup>
import {
  isPermissionGranted,
  requestPermission,
  sendNotification,
} from "@tauri-apps/plugin-notification";
import VueCountdown from "@chenfengyuan/vue-countdown";
import {CircleProgressBar} from "circle-progress.vue";
import { onMounted } from "vue";

onMounted(async () => {
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
