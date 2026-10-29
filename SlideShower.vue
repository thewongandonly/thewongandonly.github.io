<template>
  <div class="fullscreen-container">
    <!-- Background Image -->
    <Transition name="slide-right">
      <img
        v-if="isMounted"
        src="https://thewongandonly.com/dokimonika.jpg"
        alt="Full screen background"
        class="fullscreen-image"
      />
    </Transition>

    <!-- Character 1: Natsuki -->
    <div v-if="showNatsuki" class="character-wrapper natsuki-pos">
      <img
        src="https://thewongandonly.com/natsuki.png"
        alt="Natsuki"
        class="character-image dance-1"
      />
    </div>

    <!-- Character 2: Sayori -->
    <div v-if="showSayori" class="character-wrapper sayori-pos">
      <img
        src="https://thewongandonly.com/sayori.png"
        alt="Sayori"
        class="character-image dance-2"
      />
    </div>

    <!-- Character 3: Yuri -->
    <div v-if="showYuri" class="character-wrapper yuri-pos">
      <img
        src="https://thewongandonly.com/yuri.png"
        alt="Yuri"
        class="character-image dance-3"
      />
    </div>

    <div v-if="showHearts" class="hearts-container">
      <img
        v-for="n in 7"
        :key="n"
        src="https://thewongandonly.com/heart.png"
        alt="Heart"
        :class="['floating-heart', `heart-${n}`]"
      />
    </div>

  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from "vue";

const isMounted = ref(false);
const showNatsuki = ref(false);
const showSayori = ref(false);
const showYuri = ref(false);

const showHearts = ref(false)
const timers = [];

onMounted(() => {
  // Trigger background entrance
  isMounted.value = true;

  // Staggered entry sequence
  timers.push(
    setTimeout(() => {
      showNatsuki.value = true;
    }, 7000),
    setTimeout(() => {
      showSayori.value = true;
    }, 9500),
    setTimeout(() => {
      showYuri.value = true;
    }, 12000),
   setTimeout(() => { showHearts.value = true }, 13500)

  );
});

onUnmounted(() => {
  timers.forEach((t) => clearTimeout(t));
});
</script>

<style scoped>
/* Fullscreen overlay wrapper */
.fullscreen-container {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  overflow: hidden;

  z-index: 99;
}

/* Background image styling */
.fullscreen-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}

/* Background Slide-Right Transition */
.slide-right-enter-active {
  transition: transform 9.8s cubic-bezier(0.16, 1, 0.3, 1);
}

.slide-right-enter-from {
  transform: translateX(-100%);
}

.slide-right-enter-to {
  transform: translateX(0);
}

/* Shared Character Positioning & Wrapper */
.character-wrapper {
  position: absolute;
  bottom: 5%;
  z-index: 10;
  animation-duration: 1.5s;
  animation-timing-function: cubic-bezier(0.25, 1, 0.5, 1);
  animation-fill-mode: forwards;
}

/* Individual Position Targets (Side-by-side near center) */
.natsuki-pos {
  animation-name: slide-to-left-center;
}

.sayori-pos {
  animation-name: slide-to-center;
}

.yuri-pos {
  animation-name: slide-to-right-center;
}

/* Base Character Image Styling */
.character-image {
  max-height: 65vh;
  width: auto;
  display: block;
  transform-origin: bottom center;
  animation: happy-walk-wobble 0.6s ease-in-out infinite alternate;
}

/* Staggered animation delays on dance phase so they hop in natural rhythm */
.dance-1 {
  animation-delay: 0s;
}
.dance-2 {
  animation-delay: 0.2s;
}
.dance-3 {
  animation-delay: 0.4s;
}

/* Slide Keyframes */
@keyframes slide-to-left-center {
  0% {
    left: -30%;
  }
  100% {
    left: 32%;
  }
}

@keyframes slide-to-center {
  0% {
    left: -30%;
  }
  100% {
    left: 45%;
  }
}

@keyframes slide-to-right-center {
  0% {
    left: -30%;
  }
  100% {
    left: 58%;
  }
}

/* Happy Walking / Dancing Wobble */
@keyframes happy-walk-wobble {
  0% {
    transform: translateY(0) rotate(-4deg);
  }
  50% {
    transform: translateY(-18px) rotate(0deg);
  }
  100% {
    transform: translateY(0) rotate(4deg);
  }
}

/* Shared Character Positioning & Wrapper */
.character-wrapper {
  position: absolute;
  bottom: 5%;
  z-index: 10;
  animation-duration: 1.5s;
  animation-timing-function: cubic-bezier(0.25, 1, 0.5, 1);
  animation-fill-mode: forwards;
}

/* Individual Position Targets (Side-by-side near center) */
.natsuki-pos {
  animation-name: slide-to-left-center;
}

.sayori-pos {
  animation-name: slide-to-center;
}

.yuri-pos {
  animation-name: slide-to-right-center;
}

/* Base Character Image Styling */
.character-image {
  max-height: 65vh;
  width: auto;
  display: block;
  transform-origin: bottom center;
  animation: happy-walk-wobble 0.6s ease-in-out infinite alternate;
}

/* Staggered animation delays on dance phase so they hop in natural rhythm */
.dance-1 {
  animation-delay: 0s;
}
.dance-2 {
  animation-delay: 0.2s;
}
.dance-3 {
  animation-delay: 0.4s;
}

/* Slide Keyframes */
@keyframes slide-to-left-center {
  0% {
    left: -30%;
  }
  100% {
    left: 32%;
  }
}

@keyframes slide-to-center {
  0% {
    left: -30%;
  }
  100% {
    left: 45%;
  }
}

@keyframes slide-to-right-center {
  0% {
    left: -30%;
  }
  100% {
    left: 58%;
  }
}

/* Happy Walking / Dancing Wobble */
@keyframes happy-walk-wobble {
  0% {
    transform: translateY(0) rotate(-4deg);
  }
  50% {
    transform: translateY(-18px) rotate(0deg);
  }
  100% {
    transform: translateY(0) rotate(4deg);
  }
}


/* --- Floating Hearts Styles --- */
.hearts-container {
  position: absolute;
  top: 15%;
  left: 32%;
  width: 35vw;
  height: 40vh;
  z-index: 20;
  pointer-events: none;
}

.floating-heart {
  position: absolute;
  bottom: 0;
  width: 32px;
  height: auto;
  opacity: 0;
  animation: butterfly-float 4s ease-in-out infinite;
}

/* Unique starting positions, durations, and delays to simulate random butterfly motion */
.heart-1 { left: 5%;  animation-delay: 0s;   animation-duration: 3.8s; transform: scale(0.8); }
.heart-2 { left: 20%; animation-delay: 0.6s; animation-duration: 4.5s; transform: scale(1.1); }
.heart-3 { left: 35%; animation-delay: 1.2s; animation-duration: 3.5s; transform: scale(0.7); }
.heart-4 { left: 50%; animation-delay: 0.3s; animation-duration: 4.2s; transform: scale(1.0); }
.heart-5 { left: 65%; animation-delay: 1.5s; animation-duration: 3.9s; transform: scale(0.9); }
.heart-6 { left: 80%; animation-delay: 0.9s; animation-duration: 4.6s; transform: scale(1.2); }
.heart-7 { left: 92%; animation-delay: 0.2s; animation-duration: 3.6s; transform: scale(0.85); }

@keyframes butterfly-float {
  0% {
    transform: translateY(0) translateX(0) scale(var(--s, 1)) rotate(0deg);
    opacity: 0;
  }
  20% {
    opacity: 0.9;
  }
  50% {
    transform: translateY(-120px) translateX(25px) rotate(15deg);
  }
  75% {
    transform: translateY(-220px) translateX(-20px) rotate(-10deg);
  }
  100% {
    transform: translateY(-320px) translateX(15px) rotate(5deg);
    opacity: 0;
  }
}
</style>
