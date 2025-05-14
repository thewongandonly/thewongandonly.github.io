<template>
  <div class="marquee">
  <div>
    <span class="wider">David Wong is a full-stack software developer skilled in C#, Vue.js and SQL, crafting robust solutions for the web.</span> 
  </div>
</div>
</template>


<style>
.wider{
  padding-left: 550px;
}
.marquee {
  height: 25px;
  width: 100%;
    z-index: 999999;
    color: #999999;
    font-family: "Montserrat", sans-serif;
  overflow: hidden;
  position: relative;
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
