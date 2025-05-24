<template>
  <div class="marquee">
  <div>
    <span class="wider">{{msg}}</span> 
  </div>
</div>
</template>

<script>
export default {
data: function () {
  return {
    msg: "Hi, Ik ben een full-stack software ontwikkelaar uit Utrecht, Nederland. Mijn ervaring is zo hoog as de Dom!"
  }
}
};
</script>

<style>
.wider{
  padding-left: 1450px;
  padding-right: 550px;
}
.marquee {
  height: 25px;
  width: 100%;
  z-index: 999999;
  color: #999999;
  font-family: "Montserrat", sans-serif;
  overflow: hidden;
  position: fixed;
  bottom: 0;
}

.marquee div {
  display: block;
  width: 200%;
  height: 20px;

  position: absolute;
  overflow: hidden;

  animation: marquee 25s linear infinite;
}

.marquee span {
  float: left;
  width: 50%;
}

@keyframes marquee {
  0% { left: 0; }
  100% { left: -100%; }
}
</style>
