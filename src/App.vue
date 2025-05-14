<template>
  <div
    id="container"
    class="w-full h-screen bg-sc-orange/80 bg-linear-to-r from-sc-orange to-sc-orange/80 bg-[url(assets/bg-4.jpg)] bg-center bg-blend-multiply bg-cover bg-no-repeat relative flex flex-col items-center justify-center px-8"
  >
    <FlipCountdown
      class="font-medium text-9xl"
      :showDays="false"
      :deadlineDate="deadline"
      countdownSize="3.75rem"
      labelSize="1.8rem"
      @timeElapsed="onCountdownEnd"
    />
    <div
      class="size-12 absolute right-6 bottom-6 bg-sc-dark rounded-full p-2 cursor-pointer shadow"

    >
      <Cog6ToothIcon class="size-8 text-sc-orange" />
    </div>
  </div>
</template>

<script lang="ts" setup>
import { ref } from "vue";
import { Cog6ToothIcon } from "@heroicons/vue/20/solid";
import { Countdown as FlipCountdown } from "vue3-flip-countdown";
import { useSound } from "@vueuse/sound";

import sessionOverSrc from "./assets/session-over.mp3";
import breakOverSrc from "./assets/break-over.mp3";
import alarmSoundSrc from "./assets/alarm.mp3";

const SESSION_DURATION = 6 * 1000;
const BREAK_DURATION = 6 * 1000;

const deadline = ref(new Date(Date.now() + SESSION_DURATION));
const isSession = ref(false);
const isBreak = ref(true);

const { play: notifySessionOver } = useSound(sessionOverSrc);
const { play: notifyBreakOver } = useSound(breakOverSrc);
const { play: alarm } = useSound(alarmSoundSrc);

const onCountdownEnd = () => {
  if (isSession.value) {
    alarm();
    notifySessionOver();

    isBreak.value = true;
    isSession.value = false;

    window.setTimeout(() => {
      deadline.value = new Date(Date.now() + BREAK_DURATION);
    }, BREAK_DURATION);
  } else if (isBreak.value) {
    alarm();
    notifyBreakOver();

    isBreak.value = false;
    isSession.value = true;
    deadline.value = new Date(Date.now() + SESSION_DURATION);
  }
};
</script>
