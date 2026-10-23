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
      theta: 0, // Angle tracking from 0 to PI/2 (0 to 90 degrees)
      x: 0,
      y: 0,
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
        this.theta = 0;
        this.x = 0;
        this.y = 0;
        this.animate();
      } else {
        // Stop and reset immediately if clicked mid-flight
        this.isMoving = false;
        cancelAnimationFrame(this.animationId);
        this.theta = 0;
        this.x = 0;
        this.y = 0;
      }
    },
    animate() {
      if (!this.isMoving) return;

      // Increment angle (controls speed of the arc)
      this.theta += 0.025;

      const maxTheta = Math.PI / 2; // 90 degrees in radians

      // Stop and reset once it hits or exceeds 90 degrees
      if (this.theta >= maxTheta) {
        this.isMoving = false;
        cancelAnimationFrame(this.animationId);
        this.theta = 0;
        this.x = 0;
        this.y = 0;
        return;
      }

      // 1. Horizontal sweep across the arc span
      const horizontalRange = 400; 
      this.x = (this.theta / maxTheta) * horizontalRange - (horizontalRange / 2);

      // 2. Vertical arc: strict sine function from 0 to 90 degrees
      // Distance from center to top edge + image height + extra buffer ensures it's completely outside
      const verticalHeight = (window.innerHeight / 2) + 150;
      this.y = -Math.sin(this.theta) * verticalHeight;

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
