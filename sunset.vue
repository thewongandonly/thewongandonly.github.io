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

<script>
export default {
  name: "ProfileComponent",
  data() {
    return {
      isMoving: false,
      x: 0,
      y: 0,
      vx: 2.5, // Horizontal speed
      vy: 1.8, // Vertical speed
      animationId: null
    };
  },
  computed: {
    imgStyle() {
      return {
        transform: `translate(${this.x}px, ${this.y}px)`,
        cursor: "pointer"
      };
    }
  },
  methods: {
    toggleMovement() {
      this.isMoving = !this.isMoving;
      if (this.isMoving) {
        this.animate();
      } else {
        cancelAnimationFrame(this.animationId);
      }
    },
    animate() {
      if (!this.isMoving) return;

      // Update position coordinates
      this.x += this.vx;
      this.y += this.vy;

      // Calculate dynamic boundaries based on viewport size
      const limitX = window.innerWidth / 2 - 100;
      const limitY = window.innerHeight / 2 - 100;

      // Reverse direction on hitting screen boundaries (bounce effect)
      if (this.x > limitX || this.x < -limitX) {
        this.vx *= -1;
      }
      if (this.y > limitY || this.y < -limitY) {
        this.vy *= -1;
      }

      this.animationId = requestAnimationFrame(this.animate);
    }
  },
  // Clean up animation frame when component is destroyed
  beforeUnmount() {
    cancelAnimationFrame(this.animationId);
  },
  beforeDestroy() {
    cancelAnimationFrame(this.animationId);
  }
};
</script>

<style scoped>
.profile-img {
  object-fit: cover;
  object-position: center;
  border-radius: 50%;
  width: 8rem;
  height: 8rem;
  cursor: pointer;
  transition: transform 0.05s linear;
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
