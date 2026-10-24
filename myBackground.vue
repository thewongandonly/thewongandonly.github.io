<template>
  <video autoplay muted loop class="myVideo" ref="videoPlayer">
    <source :src="vid" type="video/mp4" />
  </video>
</template>

<script setup>
import { ref, watch } from 'vue';

const props = defineProps({
  vid: {
    type: String,
    required: true,
  },
});

const videoPlayer = ref(null);

const changeVid = (name) => {
  setTimeout(() => {
    if (videoPlayer.value) {
      videoPlayer.value.pause();
      videoPlayer.value.load();
      videoPlayer.value.play();
    }
  }, 250);
};

watch(
  () => props.vid,
  (newVal, oldVal) => {
    changeVid(newVal);
    // console.log("Prop changed: ", newVal, " | was: ", oldVal);
  }
);
</script>

<style scoped>
.myVideo {
  position: fixed;
  right: 0;
  bottom: 0;
  min-width: 100%;
  min-height: 100%;
}
</style>
