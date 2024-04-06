<template>
  <video autoplay muted loop id="myVideo">
    <source :src="vid" type="video/mp4" />
  </video>
</template>

<script>
export default {
  data() {
    return {
      vid: "darkcode.mp4",
    };
  },
};
</script>

<style scoped>
#myVideo {
  position: fixed;
  right: 0;
  bottom: 0;
  min-width: 100%;
  min-height: 100%;
}
</style>
