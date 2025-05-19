<template>
  <div
    id="container"
    class="w-full h-screen bg-sc-orange/80 bg-linear-to-r from-sc-orange to-sc-orange/80 bg-[url(assets/bg-4.jpg)] bg-center bg-blend-multiply bg-cover bg-no-repeat relative flex flex-col items-center justify-center px-8"
  >
    <FlipCountdown
      class="font-medium text-3xl"
      :showDays="false"
      :deadlineDate="deadline"
      countdownSize="2rem"
      labelSize="16px"
      @timeElapsed="onCountdownEnd"
    />

    <div
      class="size-8 absolute right-4 bottom-4 bg-sc-dark rounded-full p-2 cursor-pointer shadow flex items-center justify-center"
    >
      <Cog6ToothIcon class="size-6 text-sc-orange" />
    </div>
  </div>
</template>

<script lang="ts" setup>
import { ref, watch } from "vue";
import { Cog6ToothIcon } from "@heroicons/vue/20/solid";
import { Countdown as FlipCountdown } from "vue3-flip-countdown";
import { useSound } from "@vueuse/sound";

import sessionOverSrc from "./assets/session-over.mp3";
import breakOverSrc from "./assets/break-over.mp3";
import alarmSoundSrc from "./assets/alarm.mp3";

const SESSION_DURATION = 25 * 60 * 1000;
const BREAK_DURATION = 5 * 60 * 1000;

const deadline = ref(new Date(Date.now() + SESSION_DURATION));
const isSession = ref(true);
const isBreak = ref(false);

const { play: notifySessionOver } = useSound(sessionOverSrc);
const { play: notifyBreakOver } = useSound(breakOverSrc);
const { play: alarm } = useSound(alarmSoundSrc);

const onCountdownEnd = () => {
  // while (!isBreak.value && isSession.value) {
  if (isSession.value) {
    alarm();
    notifySessionOver();

    isBreak.value = true;
    isSession.value = false;

    window.setTimeout(function () {
      isBreak.value = false;
      isSession.value = true;
      deadline.value = new Date(Date.now() + BREAK_DURATION);
      alarm();
      notifyBreakOver();
    }, BREAK_DURATION);
  }
  // }
};
</script>
