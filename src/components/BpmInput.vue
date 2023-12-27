<script setup>
import { ref, watch } from "vue";
const props = defineProps({
  bpm: String,
});
const isPaused = ref(true);
const bpm = ref(props.bpm);
const beatInterval = ref((60 / props.bpm) * 1000);

const oneBeat = new Audio("/src/assets/sounds/metronome1.mp3");
const otherBeats = new Audio("/src/assets/sounds/metronome2.mp3");
let metronomeInterval = "";
const playBeat1 = () => {
  oneBeat.currentTime = 0;
  oneBeat.play();
};

const playBeat2 = () => {
  otherBeats.currentTime = 0;
  otherBeats.play();
};
const handlePlay = () => {
  //toggle pause play
  isPaused.value = !isPaused.value;

  if (!isPaused.value) {
    play();
  } else {
    stop();
  }
};

const play = () => {
  let beatCount = 0;
  metronomeInterval = setInterval(function () {
    if (beatCount % 4 === 0) {
      // play oneBeat every 4 beats
      playBeat1();
    } else {
      // play otherBeats for the remaining 3 beats
      playBeat2();
    }

    beatCount++;

    // reset beatCount after 4 beats
    if (beatCount >= 4) {
      beatCount = 0;
    }
  }, beatInterval.value);
};
const stop = () => {
  clearInterval(metronomeInterval);
};

//update interval if bpm changes
watch(bpm, () => {
  beatInterval.value = (60 / bpm.value) * 1000;
  stop();
  if (!isPaused.value) {
    play();
  }
});
</script>

<template>
  <div class="flex flex-col items-center gap-4">
    <h2 class="text-4xl">{{ bpm }} BPM</h2>
    <input v-model="bpm" type="range" min="1" max="400" />
    <img
      class="w-12 h-12"
      @click="handlePlay()"
      :src="
        isPaused ? '/src/assets/icons/play.svg' : '/src/assets/icons/pause.svg'
      "
      alt="play button"
    />
  </div>
</template>
