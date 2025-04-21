<template>
  <div class="marquee">
  <div>
    <span>Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.</span> 
  </div>
</div>
</template>


<style>
  
.marquee {
  height: 25px;
  width: 100%;

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
