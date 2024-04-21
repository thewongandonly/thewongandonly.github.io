<template>
  <video autoplay muted loop class="myVideo" ref="videoPlayer">
    <source :src="vid" type="video/mp4" />
  </video>
</template>

<script>
export default {
  props: ["vid"],
  watch: {
    vid: function (newVal, oldVal) {
      this.changeVid(newVal)
      //console.log("Prop changed: ", newVal, " | was: ", oldVal);
    },
  },
  methods: {
    changeVid(name) {
      setTimeout(() => {
        this.$refs.videoPlayer.pause();
        this.$refs.videoPlayer.load();
        this.$refs.videoPlayer.play();
      }, 250);
    },
  },
};
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
