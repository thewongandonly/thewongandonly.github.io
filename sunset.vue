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
      t: 0, // Time / progress factor for the arc
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
      if (!this.isMoving) {
        this.isMoving = true;
        this.x = 0;
        this.y = 0;
        this.t = 0;
        this.animate();
      } else {
        // Stop and reset immediately if clicked mid-flight
        this.isMoving = false;
        cancelAnimationFrame(this.animationId);
        this.x = 0;
        this.y = 0;
      }
    },
    animate() {
      if (!this.isMoving) return;

      // Increment progress over time
      this.t += 0.025;

      // 1. Horizontal sweep: smooth back-and-forth arc motion using sine
      this.x = Math.sin(this.t * 1.2) * 180;

      // 2. Vertical arc trajectory: combines a parabolic curve (dome shape) with steady upward drift
      const arcCurve = - (Math.pow(this.x, 2) / 350); // Creates the natural arc curve
      const upwardDrift = this.t * 30;                 // Pulls the arc progressively higher

      this.y = arcCurve - upwardDrift;

      // Check if the image has cleared the top of the screen
      const topLimit = (window.innerHeight / 2) + 120;

      if (this.y < -topLimit) {
        // Stop animation and reset back to center
        this.isMoving = false;
        cancelAnimationFrame(this.animationId);
        this.x = 0;
        this.y = 0;
        return;
      }

      this.animationId = requestAnimationFrame(this.animate);
    }
  },
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
