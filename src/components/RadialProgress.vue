<template>
  <div
    class="radial-progress text-sc-faint"
    :style="'--value:' + progress + '; --size: 25rem; --thickness: 2.5rem'"
  >
    <slot />
  </div>
</template>

<script lang="ts" setup>
import { defineProps } from "vue";
defineProps({
  progress: Number,
});
</script>
