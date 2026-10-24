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
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onBeforeUnmount } from 'vue';

// Define component name if needed (optional in Vue 3 with SFC filename inference, but good practice)
defineOptions({
  name: "ProfileComponent"
});

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
    theta.value = 0;
    x.value = 0;
    y.value = 0;
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
  height: 600px;
  border: 0px solid #006100;
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
</style>
