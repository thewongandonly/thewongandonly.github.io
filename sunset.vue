<template>
  <div class="full-height">
    <div class="container">
      <div class="centered-element">
        <div class="content">
          <img 
            src="https://thewongandonly.com/davidwong.png" 
            alt="David Wong" 
            class="profile-img" 
            :style="imgStyle"
            @click="toggleMovement" 
          />


		      <div class="fade"></div>
    			<div v-if="y!=0" class="star-wars" :style="{ perspective: perspective + 'px', color: textColor }">
    				  <div :key="animKey" class="crawl" :style="crawlStyles">
              					<div class="title">
              					  <h2>{{ episodeNumber }}</h2>
              					  <h1>{{ episodeTitle }}</h1>
              					</div>
          					<p v-for="(paragraph, index) in paragraphs" :key="index">
          					  {{ paragraph }}
          					</p>
    				  </div>
    				</div>
    			  
          
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onBeforeUnmount } from 'vue';
 
defineOptions({
  name: "Sunset"
});

 const episodeNumber='Sitting';
 const episodeTitle='Dock of the bay';
 const crawlText=`
Sittin' in the mornin' sun
I'll be sittin' when the evenin' come
Watching the ships roll in
And I watch 'em roll away again, yeah

I'm sittin' on the dock of the bay
Watching the tide roll away
Ooh, I'm just sittin' on the dock of the bay
Wastin' time

I left my home in Georgia
Headed for the 'Frisco bay
'Cause I've had nothing to live for
And look like nothing's gonna come my way

So I'm just gonna sit on the dock of the bay
Watching the tide roll away
Ooh, I'm sittin' on the dock of the bay
Wastin' time

Look like nothing's gonna change
Everything still remains the same
I can't do what ten people tell me to do
So I guess I'll remain the same, yes

Sittin' here resting my bones
And this loneliness won't leave me alone
It's two thousand miles I roamed
Just to make this dock my home

Now, I'm just gonna sit at the dock of the bay
Watching the tide roll away
Ooh-wee, sittin' on the dock of the bay
Wastin' time
`;
 const duration= 60;
 const perspective= 400;
 const tiltStart=20 ;
 const tiltEnd=25;
 const textColor=  '#feda4a' 

const isPlaying = ref(false);
const animKey = ref(0);

const paragraphs = computed(() => {
  return crawlText.split('\n\n').filter(p => p.trim() !== '');
});

const crawlStyles = computed(() => ({
  '--crawl-duration': `${duration}s`,
  '--crawl-play-state': isPlaying.value ? 'running' : 'paused',
  '--tilt-start': `${tiltStart}deg`,
  '--tilt-end': `${tiltEnd}deg`
}));




// Reactive state
const isMoving = ref(false);
const theta = ref(0);
const x = ref(0);
const y = ref(0);
let animationId = null;

// Computed style for the image transform
const imgStyle = computed(() => ({
  transform: `translate(${x.value}px, ${y.value}px)`,
  cursor: "pointer"
}));

// Animation loop function
const animate = () => {
  if (!isMoving.value) return;

  // Decreased step size for significantly slower motion
  theta.value += 0.008;

  const maxTheta = Math.PI / 2; // 90 degrees in radians

  // Stop and reset once it reaches 90 degrees (outside window)
  if (theta.value >= maxTheta) {
    isMoving.value = false;
    cancelAnimationFrame(animationId);
    //theta.value = 0;
    //x.value = 0;
    //y.value = 0;
    return;
  }

  // 1. Horizontal arc sweep: Starts at 0 when theta = 0, avoiding any initial jump
  const horizontalRange = 250; 
  x.value = (theta.value / maxTheta) * horizontalRange;

  // 2. Vertical arc: Strict sine trajectory starting at 0 and moving up
  const verticalHeight = (window.innerHeight / 2) + 150;
  y.value = -Math.sin(theta.value) * verticalHeight;

  animationId = requestAnimationFrame(animate);
};

// Toggle movement handler
const toggleMovement = () => {
  if (!isMoving.value) {
    isMoving.value = true;
    theta.value = 0;
    x.value = 0;
    y.value = 0;
    animate();
  } else {
    // Stop and reset immediately if clicked mid-flight
    isMoving.value = false;
    cancelAnimationFrame(animationId);
    theta.value = 0;
    x.value = 0;
    y.value = 0;
  }
};

// Cleanup lifecycle hook (Vue 3 standard)
onBeforeUnmount(() => {
  cancelAnimationFrame(animationId);
});
</script>

<style scoped>
.profile-img {
  object-fit: cover;
  object-position: center;
  border-radius: 50%;
  width: 8rem;
  height: 8rem;
  cursor: pointer;
  user-select: none;
}

.container {
  display: grid;
  align-items: center;
  height: 75vh;
  border: 0px solid #006100;
  overflow: hidden;
    position: relative;
    width: 100%;
}

.full-height {
  position: absolute;
  height: 100vh;
  width: 100%;
  color: #f3e1d1;
  font-family: "Montserrat", sans-serif !important;
  display: flex;
  flex-direction: column;
  align-items: center;
  max-width: 100%;
  text-align: center;
}

 

.fade {
  position: relative;
  width: 100%;
  min-height: 60vh;
  top: -25px;
  background-image: linear-gradient(0deg, transparent, black 75%);
  z-index: 1;
}

.star-wars {
  display: flex;
  justify-content: center;
  position: relative;
  height: 800px;
  color: #feda4a;
  font-family: 'Montserrat', sans-serif;
  font-size: 500%;
  font-weight: 600;
  letter-spacing: 6px;
  line-height: 150%;
  perspective: 400px;
  text-align: justify;
}

.crawl {
  position: relative;
  top: 9999px;
  transform-origin: 50% 100%;
  animation: crawl var(--crawl-duration, 60s) linear infinite;
  animation-play-state: var(--crawl-play-state, running);
}

.crawl > .title {
  font-size: 90%;
  text-align: center;
}

.crawl > .title h1 {
  margin: 0 0 100px;
  text-transform: uppercase;
}

@keyframes crawl {
  0% {
    top: 0;
    transform: rotateX(var(--tilt-start, 20deg)) translateZ(0);
  }
  100% { 
    top: -6000px;
    transform: rotateX(var(--tilt-end, 25deg)) translateZ(-2500px);
  }
}

</style>
