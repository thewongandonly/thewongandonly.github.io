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
      y: 0,
      vy: 1.5, // Upward speed (lower number = slower movement)
      animationId: null
    };
  },
  computed: {
    imgStyle() {
      return {
        transform: `translate(0px, ${this.y}px)`,
        cursor: "pointer"
      };
    }
  },
  methods: {
    toggleMovement() {
      if (!this.isMoving) {
        this.isMoving = true;
        this.animate();
      } else {
        // Stop and reset immediately if clicked while moving
        this.isMoving = false;
        cancelAnimationFrame(this.animationId);
        this.y = 0;
      }
    },
    animate() {
      if (!this.isMoving) return;

      // Move upwards by decreasing the y coordinate
      this.y -= this.vy;

      // Calculate when the image has moved completely past the top of the screen
      const topLimit = (window.innerHeight / 2) + 100;

      if (this.y < -topLimit) {
        // Stop animation and reset position back to center
        this.isMoving = false;
        cancelAnimationFrame(this.animationId);
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
