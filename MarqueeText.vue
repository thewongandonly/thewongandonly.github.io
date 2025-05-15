<template>
  <div class="marquee">
  <div>
    <span class="wider">David Wong is a skilled full-stack software developer proficient in C#, Vue.js, and SQL, building robust web solutions.</span> 
  </div>
</div>
</template>


<style>
.wider{
  padding-left: 850px;
  padding-right: 250px;
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
