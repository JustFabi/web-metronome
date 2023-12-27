<script setup>
import { ref } from "vue";
const props = defineProps({
  bpm: Number,
});
const bpm = ref(props.bpm);
</script>

<template>
  <div class="flex flex-col items-center gap-4">
    <h2 class="text-4xl">{{ bpm }}</h2>
    <input v-model="bpm" type="range" min="0" max="400" />
  </div>
</template>
