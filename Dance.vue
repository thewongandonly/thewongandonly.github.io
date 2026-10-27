<template>
  <div class="dancecontainer">
    <svg
      id="Popsicle"
      ref="svgRef"
      xmlns="http://www.w3.org/2000/svg"
      xmlns:xlink="http://www.w3.org/1999/xlink"
      viewBox="0 0 700 400"
      shape-rendering="geometricPrecision"
      text-rendering="geometricPrecision"
    >
      <g class="tigger-bounce" transform="matrix(.583338 0 0 0.583338 244.143646 64.848374)">
        <!-- Wooden Popsicle Stick -->
        <path
          id="e1phkaj8n7B329"
          d="M150,330 L150,440 C150,455 165,465 180,465 C195,465 210,455 210,440 L210,330 Z"
          fill="#e6c280"
          stroke="#c49a56"
          stroke-width="6"
          stroke-linecap="round"
          stroke-linejoin="round"
        />

        <!-- Left Arm -->
        <path
          id="e1phkaj8n7B297"
          d="M105,210 C70,210 50,160 30,170 C15,178 35,215 70,230 C90,238 105,225 105,210 Z"
          fill="#ff77a9"
          stroke="#ff4d8d"
          stroke-width="4"
          stroke-linecap="round"
        />

        <!-- Right Arm -->
        <g id="e1phkaj8n7B290">
          <path
            d="M255,210 C290,210 310,160 330,170 C345,178 325,215 290,230 C270,238 255,225 255,210 Z"
            fill="#ff77a9"
            stroke="#ff4d8d"
            stroke-width="4"
            stroke-linecap="round"
          />
        </g>

        <!-- Main Body: Cute Pink Popsicle with Soft Curved Top -->
        <g id="e1phkaj8n7B289" transform="translate(0 -1.216422)">
          <rect
            x="100"
            y="70"
            width="160"
            height="270"
            rx="80"
            ry="80"
            fill="#ff8dae"
            stroke="#ff4d8d"
            stroke-width="8"
          />

          <!-- Soft Cream/White Frosting Drip Overlay -->
          <path
            d="M100,150 C120,165 135,135 155,160 C175,185 195,145 215,165 C235,185 245,155 260,150 L260,150 C260,105 224,70 180,70 C135,70 100,105 100,150 Z"
            fill="#fff5f8"
            opacity="0.9"
          />

          <!-- Glossy Highlight Curve -->
          <path
            d="M125,110 C120,140 120,240 125,280"
            fill="none"
            stroke="#ffffff"
            stroke-width="10"
            stroke-linecap="round"
            opacity="0.6"
          />

          <!-- Face Details -->
          <g transform="translate(0, 10)">
            <circle cx="150" cy="190" r="8" fill="#4a2e35" />
            <circle cx="148" cy="188" r="2.5" fill="#ffffff" />
            
            <circle cx="210" cy="190" r="8" fill="#4a2e35" />
            <circle cx="208" cy="188" r="2.5" fill="#ffffff" />

            <ellipse cx="138" cy="202" rx="10" ry="6" fill="#ff4d8d" opacity="0.6" />
            <ellipse cx="222" cy="202" rx="10" ry="6" fill="#ff4d8d" opacity="0.6" />

            <path
              d="M172,202 Q180,214 188,202"
              fill="none"
              stroke="#4a2e35"
              stroke-width="4"
              stroke-linecap="round"
            />
          </g>
        </g>
      </g>
    </svg>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const svgRef = ref(null)
</script>

<style>
.dancecontainer {
  position: relative;
  top: 100px; 
  margin-top: 120px;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 70%;
}

.dancecontainer svg {
  display: block;
  margin: 0 auto;
}

/* Higher Tigger-style bouncy spring animation */
@keyframes tiggerBounce {
  0% {
    transform: matrix(0.583338, 0, 0, 0.583338, 244.143646, 20.848374) translateY(0) scaleY(1);
  }
  30% {
    transform: matrix(0.583338, 0, 0, 0.583338, 244.143646, -10.848374) translateY(-45px) scaleY(1.05) scaleX(0.95);
  }
  50% {
    transform: matrix(0.583338, 0, 0, 0.583338, 244.143646, 30.848374) translateY(0) scaleY(0.85) scaleX(1.1);
  }
  70% {
    transform: matrix(0.583338, 0, 0, 0.583338, 244.143646, 2.848374) translateY(-20px) scaleY(1.02);
  }
  100% {
    transform: matrix(0.583338, 0, 0, 0.583338, 244.143646, 20.848374) translateY(0) scaleY(1);
  }
}

.tigger-bounce {
  animation: tiggerBounce 0.6s cubic-bezier(0.28, 0.84, 0.42, 1) infinite;
  transform-origin: 350px 200px;
}
</style>