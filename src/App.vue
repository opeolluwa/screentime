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
      @timeElapsed="updateTime()"
    />

    <div
      class="size-12 absolute right-6 bottom-6 bg-sc-orange rounded-full p-2 cursor-pointer shadow"
    >
      <Cog6ToothIcon class="size-8 text-sc-faint" />
    </div>
  </div>
</template>

<script lang="ts" setup>
import { Countdown } from "vue3-flip-countdown";
import { Cog6ToothIcon } from "@heroicons/vue/20/solid";
import { ref } from "vue";
const SESSION = 6 * 1000;
const BREAK = 5 * 60 * 1000;
const date = ref(new Date().getTime() + SESSION);

function updateTime() {
  date.value = new Date().getTime() + BREAK;
}



</script>
<style scoped></style>
