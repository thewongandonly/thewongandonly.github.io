<template>
  <div class="game-app-container">
    <!-- GAME CONTAINER -->
    <div
      ref="gameWindow"
      class="game-window"
      :class="{ shaking: speedMode === 'turbo' }"
    >
      <!-- SCENE VIEWPORT -->
      <div class="game-viewport">
        <!-- CANVAS FOR ANIMATED SEA & PARALLAX BACKGROUND -->
        <canvas ref="stageCanvas" class="stage-canvas"></canvas>

        <!-- SVG GAME ARTWORK OVERLAY (PARALLAX BOAT & DRIVER) -->
        <div class="scene-overlay">
          <!-- BOAT AND CHARACTER SVG CONTAINER -->
          <div
            class="boat-wrapper"
            :class="[
              speedMode === 'turbo' ? 'boat-fast nitro-active' : 'boat-normal',
            ]"
          >
            <svg viewBox="0 0 1000 450" class="boat-svg">
              <defs>
                <linearGradient
                  id="hullGradient"
                  x1="0%"
                  y1="0%"
                  x2="0%"
                  y2="100%"
                >
                  <stop offset="0%" stop-color="#ffffff" />
                  <stop offset="45%" stop-color="#e2e8f0" />
                  <stop offset="50%" stop-color="#00bebc" />
                  <stop offset="68%" stop-color="#008b8b" />
                  <stop offset="70%" stop-color="#ff2a8d" />
                  <stop offset="85%" stop-color="#b8165c" />
                  <stop offset="100%" stop-color="#1e152a" />
                </linearGradient>

                <linearGradient
                  id="deckGradient"
                  x1="0%"
                  y1="0%"
                  x2="100%"
                  y2="50%"
                >
                  <stop offset="0%" stop-color="#f8fafc" />
                  <stop offset="50%" stop-color="#cbd5e1" />
                  <stop offset="100%" stop-color="#64748b" />
                </linearGradient>

                <linearGradient
                  id="windshieldGrad"
                  x1="0%"
                  y1="0%"
                  x2="100%"
                  y2="100%"
                >
                  <stop offset="0%" stop-color="#ff9a9e" stop-opacity="0.8" />
                  <stop offset="50%" stop-color="#fecfef" stop-opacity="0.5" />
                  <stop offset="100%" stop-color="#6b1158" stop-opacity="0.9" />
                </linearGradient>

                <linearGradient
                  id="suitGrad"
                  x1="0%"
                  y1="0%"
                  x2="100%"
                  y2="100%"
                >
                  <stop offset="0%" stop-color="#f1f5f9" />
                  <stop offset="60%" stop-color="#cbd5e1" />
                  <stop offset="100%" stop-color="#94a3b8" />
                </linearGradient>

                <linearGradient
                  id="shirtGrad"
                  x1="0%"
                  y1="0%"
                  x2="0%"
                  y2="100%"
                >
                  <stop offset="0%" stop-color="#38bdf8" />
                  <stop offset="100%" stop-color="#0284c7" />
                </linearGradient>

                <linearGradient
                  id="skinGrad"
                  x1="0%"
                  y1="0%"
                  x2="100%"
                  y2="100%"
                >
                  <stop offset="0%" stop-color="#ffdbac" />
                  <stop offset="70%" stop-color="#f1c27d" />
                  <stop offset="100%" stop-color="#e0ac69" />
                </linearGradient>

                <linearGradient
                  id="flameGrad"
                  x1="0%"
                  y1="50%"
                  x2="100%"
                  y2="50%"
                >
                  <stop offset="0%" stop-color="#ffffff" />
                  <stop offset="30%" stop-color="#ffea00" />
                  <stop offset="70%" stop-color="#ff4400" />
                  <stop offset="100%" stop-color="#ff0055" stop-opacity="0" />
                </linearGradient>

                <filter
                  id="vectorShadow"
                  x="-10%"
                  y="-10%"
                  width="130%"
                  height="130%"
                >
                  <feDropShadow
                    dx="3"
                    dy="4"
                    stdDeviation="0"
                    flood-color="#0b0813"
                    flood-opacity="0.9"
                  />
                </filter>
              </defs>

              <!-- NITRO EXHAUST FLAMES -->
              <g
                class="nitro-group"
                :class="{ 'flames-hidden': speedMode !== 'turbo' }"
              >
                <polygon
                  points="120,285 20,270 90,295 10,310 120,305"
                  fill="url(#flameGrad)"
                >
                  <animate
                    attributeName="opacity"
                    values="0.7;1;0.8;1"
                    dur="0.1s"
                    repeatCount="indefinite"
                  />
                </polygon>
                <polygon
                  points="110,295 0,290 80,310 -20,330 110,320"
                  fill="url(#flameGrad)"
                >
                  <animate
                    attributeName="opacity"
                    values="1;0.6;0.9;0.7"
                    dur="0.12s"
                    repeatCount="indefinite"
                  />
                </polygon>
              </g>

              <!-- DUAL ENGINE EXHAUST PIPES -->
              <g stroke="#1e293b" stroke-width="2">
                <rect
                  x="110"
                  y="280"
                  width="22"
                  height="12"
                  rx="2"
                  fill="#64748b"
                />
                <circle cx="114" cy="286" r="4" fill="#0f172a" />
                <rect
                  x="100"
                  y="295"
                  width="22"
                  height="12"
                  rx="2"
                  fill="#475569"
                />
                <circle cx="104" cy="301" r="4" fill="#0f172a" />
              </g>

              <!-- BOAT HULL BASE SHADOW -->
              <path
                d="M 120 330 L 890 330 L 980 345 L 850 365 L 140 360 Z"
                fill="#051322"
                opacity="0.6"
              />

              <!-- MAIN BOAT HULL -->
              <path
                d="M 130 265 L 420 250 L 450 280 L 120 295 Z"
                fill="#cbd5e1"
                stroke="#0f172a"
                stroke-width="4"
              />
              <path
                d="M 130 265 L 350 255 L 360 270 L 120 280 Z"
                fill="#f8fafc"
              />

              <!-- Engine Scoop Grill -->
              <polygon
                points="200,256 310,251 320,263 210,268"
                fill="#1e293b"
                stroke="#475569"
                stroke-width="2"
              />
              <line
                x1="230"
                y1="255"
                x2="237"
                y2="266"
                stroke="#ff2a8d"
                stroke-width="3"
              />
              <line
                x1="250"
                y1="254"
                x2="257"
                y2="265"
                stroke="#00bebc"
                stroke-width="3"
              />
              <line
                x1="270"
                y1="253"
                x2="277"
                y2="264"
                stroke="#ff2a8d"
                stroke-width="3"
              />

              <!-- DRIVER: Bong Tubs -->
              <g class="interactive-object" @click="interactBong">
                <path
                  d="M 400 220 C 400 190, 425 185, 440 185 C 455 185, 465 200, 465 240 Z"
                  fill="#007a78"
                  stroke="#0f172a"
                  stroke-width="3"
                />
                <path
                  d="M 410 240 Q 425 210 460 212 L 515 255 L 450 275 Z"
                  fill="url(#suitGrad)"
                  stroke="#1e293b"
                  stroke-width="3.5"
                />
                <path
                  d="M 460 215 L 525 228 L 545 250 L 510 255 Z"
                  fill="#e2e8f0"
                  stroke="#1e293b"
                  stroke-width="3"
                />
                <path
                  d="M 525 228 L 565 235 L 570 248 L 545 250 Z"
                  fill="url(#skinGrad)"
                  stroke="#1e293b"
                  stroke-width="2.5"
                />
                <path
                  d="M 480 214 L 505 218 L 498 245 L 478 240 Z"
                  fill="url(#shirtGrad)"
                  stroke="#0f172a"
                  stroke-width="2"
                />

                <!-- Head & Face -->
                <path
                  d="M 470 198 L 488 200 L 485 216 L 467 214 Z"
                  fill="#e0ac69"
                  stroke="#1e293b"
                  stroke-width="2"
                />
                <path
                  d="M 462 170 C 455 155, 485 145, 500 162 C 512 175, 505 195, 488 202 C 472 205, 460 190, 462 170 Z"
                  fill="url(#skinGrad)"
                  stroke="#1e293b"
                  stroke-width="3.5"
                />
                <path
                  d="M 475 185 Q 495 188 498 198 C 490 205, 470 203, 470 192 Z"
                  fill="#ab8154"
                  opacity="0.6"
                />
                <ellipse
                  cx="468"
                  cy="180"
                  rx="4"
                  ry="6"
                  fill="#e0ac69"
                  stroke="#1e293b"
                  stroke-width="2"
                />
                <path
                  d="M 458 175 C 450 160, 460 140, 485 142 C 505 140, 515 155, 508 168 C 498 152, 475 150, 462 168 Z"
                  fill="#d97706"
                  stroke="#1e293b"
                  stroke-width="3"
                />
                <path
                  d="M 470 144 C 485 148, 500 156, 505 165"
                  stroke="#fef08a"
                  stroke-width="3"
                  stroke-linecap="round"
                  fill="none"
                />

                <!-- Sunglasses -->
                <path
                  d="M 482 170 L 506 172 L 504 183 L 484 180 Z"
                  fill="#09080e"
                  stroke="#fbbf24"
                  stroke-width="2"
                />
                <path
                  d="M 494 171 L 494 174"
                  stroke="#fbbf24"
                  stroke-width="2"
                />
                <line
                  x1="485"
                  y1="173"
                  x2="492"
                  y2="180"
                  stroke="#ffffff"
                  stroke-width="2"
                  stroke-linecap="round"
                />
                <line
                  x1="497"
                  y1="174"
                  x2="502"
                  y2="180"
                  stroke="#ffffff"
                  stroke-width="1.5"
                  stroke-linecap="round"
                  stroke-opacity="0.8"
                />

                <!-- Hands -->
                <ellipse
                  cx="572"
                  cy="242"
                  rx="7"
                  ry="6"
                  fill="#f1c27d"
                  stroke="#1e293b"
                  stroke-width="2"
                />
              </g>

              <!-- DASHBOARD & WINDSHIELD -->
              <ellipse
                cx="578"
                cy="245"
                rx="5"
                ry="18"
                fill="none"
                stroke="#334155"
                stroke-width="4.5"
              />
              <line
                x1="578"
                y1="245"
                x2="568"
                y2="245"
                stroke="#94a3b8"
                stroke-width="3"
              />
              <path
                d="M 440 248 L 520 185 L 630 188 L 660 240 Z"
                fill="url(#windshieldGrad)"
                stroke="#0f172a"
                stroke-width="4"
              />
              <path d="M 520 185 L 530 242" stroke="#1e293b" stroke-width="3" />
              <path
                d="M 445 242 L 515 190"
                stroke="#ffffff"
                stroke-width="3"
                stroke-linecap="round"
                opacity="0.6"
              />

              <!-- HULL BODY -->
              <path
                d="M 120 295 L 450 280 L 660 240 L 980 325 L 880 335 L 120 300 Z"
                fill="url(#deckGradient)"
                stroke="#0f172a"
                stroke-width="4"
              />
              <path
                d="M 115 295 L 880 335 L 980 325 L 910 345 L 160 338 Z"
                fill="url(#hullGradient)"
                stroke="#0f172a"
                stroke-width="4"
                filter="url(#vectorShadow)"
              />

              <!-- STRIPES -->
              <path
                d="M 180 310 L 850 332 L 910 330 L 820 338 L 175 320 Z"
                fill="#ff2a8d"
              />
              <path
                d="M 220 304 L 800 327 L 860 326 L 780 331 L 215 311 Z"
                fill="#00bebc"
              />
              <polygon
                points="300,300 700,320 730,323 290,305"
                fill="#ffda79"
              />

              <text
                x="320"
                y="318"
                class="boat-title-text"
                transform="rotate(1.5, 320, 318)"
                letter-spacing="2"
              >
                SCARAB 38
              </text>

              <path
                d="M 660 238 L 965 320"
                stroke="#f8fafc"
                stroke-width="4"
                stroke-linecap="round"
              />
              <path
                d="M 720 252 L 720 262 M 800 274 L 800 284 M 880 297 L 880 307"
                stroke="#94a3b8"
                stroke-width="2.5"
              />

              <!-- WATER SPRAY -->
              <g>
                <path
                  d="M 920 330 Q 970 310 1000 260 Q 960 320 900 340 Z"
                  fill="#ffffff"
                  opacity="0.85"
                >
                  <animate
                    attributeName="d"
                    values="M 920 330 Q 970 310 1000 260 Q 960 320 900 340 Z; M 920 330 Q 980 300 1010 240 Q 950 325 900 340 Z; M 920 330 Q 970 310 1000 260 Q 960 320 900 340 Z"
                    dur="0.2s"
                    repeatCount="indefinite"
                  />
                </path>
                <path
                  d="M 880 335 Q 940 330 980 290 Q 930 340 860 345 Z"
                  fill="#66e4e8"
                  opacity="0.7"
                >
                  <animate
                    attributeName="d"
                    values="M 880 335 Q 940 330 980 290 Q 930 340 860 345 Z; M 880 335 Q 950 320 990 280 Q 920 345 860 345 Z; M 880 335 Q 940 330 980 290 Q 930 340 860 345 Z"
                    dur="0.25s"
                    repeatCount="indefinite"
                  />
                </path>
              </g>

              <!-- REAR WAKE FOAM -->
              <g>
                <path
                  d="M 160 335 Q 90 320 10 360 Q 90 350 170 345 Z"
                  fill="#ffffff"
                  opacity="0.8"
                />
                <path
                  d="M 200 340 Q 110 335 30 390 Q 110 365 210 350 Z"
                  fill="#55e6c1"
                  opacity="0.5"
                />
              </g>
            </svg>
          </div>
        </div>

        <!-- HOTSPOT OVERLAY -->
        <div class="hud-location">
          <div class="hud-location-box">
            <span class="ping-dot"></span>
            <span class="location-text">BISCAYNE BAY - 1986</span>
          </div>
        </div>

        <!-- GAME DIALOG OVERLAY -->
        <div class="dialog-container" :class="{ 'dialog-hidden': !showDialog }">
          <span class="dialog-box"> "{{ currentQuote }}" </span>
        </div>

        <!-- SPEEDOMETER HUD OVERLAY -->
        <div class="speedometer">
          <div class="speed-text-container">
            <div class="speed-label">SPEED</div>
            <div class="speed-value">{{ Math.round(currentSpeed) }} KTS</div>
          </div>
          <div class="speedometer-gauge">
            <div
              class="needle"
              :style="{ transform: `rotate(${needleAngle}deg)` }"
            ></div>
            <div class="gauge-center"></div>
          </div>
        </div>
      </div>

      <!-- 90s SCUMM / FULL THROTTLE INTERFACE PANEL -->
      <div class="scumm-panel">
        <!-- ACTION TEXT LINE -->
        <div class="action-bar">
          <div class="action-text">
            {{ actionText }}
          </div>
          <div class="audio-toggle-container">
            <button
              @click="toggleAudio"
              class="audio-btn"
              :class="isAudioOn ? 'audio-on' : 'audio-off'"
            >
              🔊 AUDIO: {{ isAudioOn ? "ON" : "OFF" }}
            </button>
          </div>
        </div>

        <!-- SCUMM COMMAND GRID & CONTROLS -->
        <div class="scumm-controls-grid">
          <!-- VERB BUTTONS -->
          <div class="verbs-grid">
            <button
              v-for="verb in verbs"
              :key="verb"
              @click="selectVerb(verb)"
              class="scumm-btn"
              :class="{ active: selectedVerb === verb }"
            >
              {{ verb }}
            </button>
          </div>

          <!-- THROTTLE & SPECIAL ACTIONS -->
          <div class="engine-controls">
            <div class="engine-header">
              <span class="engine-title">SCARAB V8 TWIN TURBO ENGINE</span>
              <span
                class="mode-badge"
                :class="speedMode === 'turbo' ? 'badge-turbo' : 'badge-cruise'"
              >
                {{ speedMode === "turbo" ? "TURBO NITRO" : "CRUISE" }}
              </span>
            </div>

            <!-- ACTION CONTROL BUTTONS -->
            <div class="action-buttons-row">
              <button
                @click="setSpeed('cruise')"
                class="scumm-btn engine-btn"
                :class="{ active: speedMode === 'cruise' }"
              >
                <span>⚓ CRUISE</span>
              </button>
              <button
                @click="setSpeed('turbo')"
                class="scumm-btn engine-btn turbo-action-btn"
                :class="{ active: speedMode === 'turbo' }"
              >
                <span>🔥 TURBO</span>
              </button>
              <button
                @click="triggerBongTalk"
                class="scumm-btn engine-btn talk-action-btn"
              >
                <span>💬 TALK</span>
              </button>
            </div>

            <!-- STATUS HELPER -->
            <div class="status-footer">
              <span>Click Bong or Boat to interact</span>
              <span class="scumm-version">LUCASARTS SCUMM v5.2</span>
            </div>
          </div>
        </div>
      </div>

      <!-- SCANLINE OVERLAY -->
      <div class="scanlines"></div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted, onUnmounted } from "vue";

// Refs & Reactive State
const stageCanvas = ref(null);
const gameWindow = ref(null);

const speedMode = ref("cruise");
const targetSpeed = ref(65);
const currentSpeed = ref(65);
const selectedVerb = ref("Use");
const actionText = ref("Walk to speedboat");
const showDialog = ref(false);
const currentQuote = ref("");

const isAudioOn = ref(false);
let audioCtx = null;
let engineOsc = null;
let engineGain = null;
let animationFrameId = null;

const verbs = [
  "Give",
  "Open",
  "Close",
  "Pick up",
  "Look at",
  "Talk to",
  "Use",
  "Push",
  "Pull",
];

const quotes = [
  "We gotta catch Tubbs before the offshore drop.",
  "This Scarab 38 KV handles like a dream.",
  "Keep your eyes peeled for Vice command.",
  "No time for a coffee break, partner.",
  "I love the smell of ocean salt and race fuel in the evening.",
];
let quoteIdx = 0;

const needleAngle = computed(() => {
  return -45 + ((currentSpeed.value - 40) / 80) * 180;
});

// Canvas Parallax & Render logic
let ctx = null;
let width = 0;
let height = 0;
let time = 0;

const stars = [];
const palmTrees = [];
const cityBuildings = [];
const waterParticles = [];

class SprayParticle {
  constructor() {
    this.reset();
  }
  reset() {
    const isTurbo = speedMode.value === "turbo";
    this.x = width * 0.75 + (Math.random() * 40 - 20);
    this.y = height * 0.72 + Math.random() * 10;
    this.vx = Math.random() * 4 + (isTurbo ? 10 : 5);
    this.vy = -(Math.random() * 3 + (isTurbo ? 6 : 3));
    this.size = Math.random() * (isTurbo ? 6 : 4) + 2;
    this.alpha = 1;
    this.color = Math.random() > 0.3 ? "#ffffff" : "#66e4e8";
  }
  update() {
    this.x += this.vx;
    this.y += this.vy;
    this.vy += 0.25;
    this.alpha -= 0.03;
    if (this.alpha <= 0) this.reset();
  }
  draw(ctx) {
    ctx.save();
    ctx.globalAlpha = Math.max(0, this.alpha);
    ctx.fillStyle = this.color;
    ctx.fillRect(this.x, this.y, this.size, this.size);
    ctx.restore();
  }
}

function initBackgroundElements() {
  stars.length = 0;
  for (let i = 0; i < 60; i++) {
    stars.push({
      x: Math.random() * width,
      y: Math.random() * (height * 0.4),
      size: Math.random() * 2 + 1,
      alpha: Math.random(),
    });
  }

  cityBuildings.length = 0;
  let currentX = 0;
  while (currentX < width + 100) {
    const bWidth = 25 + Math.random() * 45;
    const bHeight = 40 + Math.random() * 70;
    cityBuildings.push({
      x: currentX,
      w: bWidth,
      h: bHeight,
      type: Math.floor(Math.random() * 3),
    });
    currentX += bWidth + Math.random() * 15;
  }

  palmTrees.length = 0;
  for (let i = 0; i < 5; i++) {
    palmTrees.push({
      x: (width / 4) * i + Math.random() * 100,
      scale: 0.5 + Math.random() * 0.5,
    });
  }

  waterParticles.length = 0;
  for (let i = 0; i < 40; i++) {
    waterParticles.push(new SprayParticle());
  }
}

function resizeCanvas() {
  if (!stageCanvas.value) return;
  width = stageCanvas.value.parentElement.clientWidth;
  height = stageCanvas.value.parentElement.clientHeight;
  stageCanvas.value.width = width;
  stageCanvas.value.height = height;
  initBackgroundElements();
}

function drawPalmTree(ctx, x, y, scale) {
  ctx.save();
  ctx.translate(x, y);
  ctx.scale(scale, scale);
  ctx.fillStyle = "#0a0314";

  ctx.beginPath();
  ctx.moveTo(-5, 0);
  ctx.quadraticCurveTo(-15, -40, -8, -80);
  ctx.quadraticCurveTo(0, -40, 5, 0);
  ctx.fill();

  for (let a = 0; a < 5; a++) {
    ctx.save();
    ctx.translate(-8, -80);
    ctx.rotate(((a * 40 - 80) * Math.PI) / 180);
    ctx.beginPath();
    ctx.moveTo(0, 0);
    ctx.quadraticCurveTo(25, -10, 45, 15);
    ctx.quadraticCurveTo(20, 5, 0, 0);
    ctx.fill();
    ctx.restore();
  }

  ctx.restore();
}

function renderScene() {
  if (!ctx) return;
  time += speedMode.value === "turbo" ? 0.08 : 0.03;

  const horizonY = height * 0.52;
  const skyGrad = ctx.createLinearGradient(0, 0, 0, horizonY);
  skyGrad.addColorStop(0, "#120524");
  skyGrad.addColorStop(0.4, "#4a0e4e");
  skyGrad.addColorStop(0.75, "#b8165c");
  skyGrad.addColorStop(1, "#ff6b4a");
  ctx.fillStyle = skyGrad;
  ctx.fillRect(0, 0, width, horizonY);

  ctx.fillStyle = "#ffffff";
  stars.forEach((star) => {
    ctx.globalAlpha = 0.3 + Math.sin(time * 2 + star.x) * 0.4;
    ctx.fillRect(star.x, star.y, star.size, star.size);
  });
  ctx.globalAlpha = 1.0;

  const sunRadius = Math.min(width, height) * 0.18;
  const sunX = width * 0.3;
  const sunY = horizonY - sunRadius * 0.3;

  const sunGrad = ctx.createLinearGradient(
    sunX,
    sunY - sunRadius,
    sunX,
    sunY + sunRadius,
  );
  sunGrad.addColorStop(0, "#ffe600");
  sunGrad.addColorStop(0.5, "#ff2a8d");
  sunGrad.addColorStop(1, "#b8165c");

  ctx.save();
  ctx.fillStyle = sunGrad;
  ctx.beginPath();
  ctx.arc(sunX, sunY, sunRadius, 0, Math.PI * 2);
  ctx.fill();

  ctx.fillStyle = "#120524";
  const numBlinds = 6;
  for (let i = 0; i < numBlinds; i++) {
    const blindY = sunY + i * (sunRadius / numBlinds) - 5;
    const blindHeight = 2 + i * 1.5;
    ctx.fillRect(
      sunX - sunRadius - 10,
      blindY,
      sunRadius * 2 + 20,
      blindHeight,
    );
  }
  ctx.restore();

  const citySpeed = (time * 15) % width;
  ctx.fillStyle = "#1e0838";
  cityBuildings.forEach((b) => {
    let drawX = (b.x - citySpeed) % (width + 100);
    if (drawX < -100) drawX += width + 100;
    ctx.fillRect(drawX, horizonY - b.h, b.w, b.h);
    ctx.fillStyle = "#ffda79";
    if (b.type === 1) {
      ctx.fillRect(drawX + 5, horizonY - b.h + 10, 4, 4);
      ctx.fillRect(drawX + 15, horizonY - b.h + 25, 4, 4);
    }
    ctx.fillStyle = "#1e0838";
  });

  const palmSpeed = (time * 40) % (width + 200);
  palmTrees.forEach((palm) => {
    let px = palm.x - palmSpeed;
    if (px < -100) palm.x += width + 200;
    drawPalmTree(ctx, px, horizonY, palm.scale);
  });

  const oceanGrad = ctx.createLinearGradient(0, horizonY, 0, height);
  oceanGrad.addColorStop(0, "#0a243a");
  oceanGrad.addColorStop(0.3, "#094359");
  oceanGrad.addColorStop(0.7, "#086375");
  oceanGrad.addColorStop(1, "#031c28");
  ctx.fillStyle = oceanGrad;
  ctx.fillRect(0, horizonY, width, height - horizonY);

  const waveCount = 14;
  const oceanHeight = height - horizonY;

  for (let i = 0; i < waveCount; i++) {
    const progress = i / waveCount;
    const y = horizonY + Math.pow(progress, 2.2) * oceanHeight;
    const waveSpeed = time * (1 + progress * 4);
    const waveShift = Math.sin(waveSpeed + i) * (10 + progress * 30);

    ctx.strokeStyle = i % 2 === 0 ? "#18a999" : "#00bebc";
    ctx.lineWidth = 1 + progress * 3.5;

    ctx.beginPath();
    ctx.moveTo(0, y);
    for (let x = 0; x < width; x += 40) {
      const waveY = y + Math.sin(x * 0.01 + waveSpeed) * (2 + progress * 6);
      ctx.lineTo(x, waveY);
    }
    ctx.lineTo(width, y);
    ctx.stroke();

    if (progress > 0.1) {
      ctx.fillStyle = i % 2 === 0 ? "#ff2a8d" : "#ffda79";
      ctx.globalAlpha = 0.4 - progress * 0.25;
      const reflWidth = 100 + progress * 300;
      ctx.fillRect(
        sunX - reflWidth / 2 + waveShift,
        y,
        reflWidth,
        2 + progress * 3,
      );
      ctx.globalAlpha = 1.0;
    }
  }

  waterParticles.forEach((p) => {
    p.update();
    p.draw(ctx);
  });

  currentSpeed.value += (targetSpeed.value - currentSpeed.value) * 0.05;

  animationFrameId = requestAnimationFrame(renderScene);
}

// Gameplay Interactivity Logic
function setSpeed(mode) {
  speedMode.value = mode;
  if (mode === "turbo") {
    targetSpeed.value = 115;
    actionText.value = "Engage twin-turbo nitro boosters!";
    playEngineSound(1.8);
  } else {
    targetSpeed.value = 65;
    actionText.value = "Cruising through Biscayne Bay";
    playEngineSound(1.0);
  }
}

function selectVerb(verb) {
  selectedVerb.value = verb;
  actionText.value = `${verb} speedboat`;
}

let dialogTimeout = null;
function triggerBongTalk() {
  currentQuote.value = quotes[quoteIdx];
  quoteIdx = (quoteIdx + 1) % quotes.length;
  showDialog.value = true;
  playVoiceBeep();

  if (dialogTimeout) clearTimeout(dialogTimeout);
  dialogTimeout = setTimeout(() => {
    showDialog.value = false;
  }, 3500);
}

function interactBong() {
  actionText.value = `${selectedVerb.value} Bong Ocean`;
  triggerBongTalk();
}

// Procedural Audio Synthesizer
function initAudio() {
  if (audioCtx) return;
  const AudioContext = window.AudioContext || window.webkitAudioContext;
  audioCtx = new AudioContext();

  engineOsc = audioCtx.createOscillator();
  const engineFilter = audioCtx.createBiquadFilter();
  engineGain = audioCtx.createGain();

  engineOsc.type = "sawtooth";
  engineOsc.frequency.setValueAtTime(55, audioCtx.currentTime);

  engineFilter.type = "lowpass";
  engineFilter.frequency.setValueAtTime(220, audioCtx.currentTime);

  engineGain.gain.setValueAtTime(0.05, audioCtx.currentTime);

  engineOsc.connect(engineFilter);
  engineFilter.connect(engineGain);
  engineGain.connect(audioCtx.destination);
  engineOsc.start();
}

function toggleAudio() {
  if (!isAudioOn.value) {
    initAudio();
    if (audioCtx && audioCtx.state === "suspended") {
      audioCtx.resume();
    }
    isAudioOn.value = true;
  } else {
    if (audioCtx) audioCtx.suspend();
    isAudioOn.value = false;
  }
}

function playEngineSound(multiplier) {
  if (!audioCtx || !isAudioOn.value) return;
  engineOsc.frequency.setTargetAtTime(
    55 * multiplier,
    audioCtx.currentTime,
    0.1,
  );
}

function playVoiceBeep() {
  if (!audioCtx || !isAudioOn.value) return;
  const now = audioCtx.currentTime;
  for (let i = 0; i < 6; i++) {
    const osc = audioCtx.createOscillator();
    const gain = audioCtx.createGain();
    osc.type = "square";
    osc.frequency.setValueAtTime(280 + Math.random() * 120, now + i * 0.08);
    gain.gain.setValueAtTime(0.04, now + i * 0.08);
    gain.gain.exponentialRampToValueAtTime(0.001, now + i * 0.08 + 0.05);
    osc.connect(gain);
    gain.connect(audioCtx.destination);
    osc.start(now + i * 0.08);
    osc.stop(now + i * 0.08 + 0.05);
  }
}

function handleFirstClick() {
  if (audioCtx && audioCtx.state === "suspended" && isAudioOn.value) {
    audioCtx.resume();
  }
}

onMounted(() => {
  if (stageCanvas.value) {
    ctx = stageCanvas.value.getContext("2d");
  }
  resizeCanvas();
  renderScene();
  window.addEventListener("resize", resizeCanvas);
  document.body.addEventListener("click", handleFirstClick, { once: true });

  // Set initial value on mount
  isMobile.value = mediaQuery.matches;
  // Listen for window resize/breakpoint changes
  mediaQuery.addEventListener("change", updateScreenSize);
});

onUnmounted(() => {
  if (animationFrameId) cancelAnimationFrame(animationFrameId);
  window.removeEventListener("resize", resizeCanvas);
  document.body.removeEventListener("click", handleFirstClick);
  if (audioCtx) {
    audioCtx.close();
  }
  // Remove media query listener on unmount
  mediaQuery.removeEventListener("change", updateScreenSize);
});

// 1. Create a reactive reference for the screen width condition
const isMobile = ref(false);

// Define your mobile breakpoint (e.g., 768px for standard tablets/phones)
const mediaQuery = window.matchMedia("(max-width: 767px)");

// 2. Update the reference when the match state changes
const updateScreenSize = (e) => {
  isMobile.value = e.matches;
};

// 3. Use it just like a computed property or reactive state
const dynamicClass = computed(() => {
  return isMobile.value ? "mobile-layout" : "desktop-layout";
});
</script>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=VT323&family=Press+Start+2P&family=Outfit:wght@600;800&display=swap");



/* Base Container Styling */
.game-app-container {
  display: none; /* Hidden by default on mobile */
  flex-direction: column;
  justify-content: space-between;
  align-items: center;

  background-color: #030712;
  font-family: "VT323", monospace;
  overflow: hidden;
  user-select: none;
  -webkit-user-select: none;
}
/* Shown on screens wider than 768px (Desktop/Tablet) */
@media (min-width: 768px) {
  .game-app-container {
    display: flex; /* or flex, grid, etc. */
  }
}
/* Game Window Container */
.game-window {
  position: relative;
  width: 100%;

  aspect-ratio: 16 / 10;
  max-height: 82vh;
  background-color: #000000;
  border-radius: 0.5rem;
  overflow: hidden;
  border: 4px solid #1e293b;
  display: flex;
  flex-direction: column;
}

/* CRT & Scanline Effects */
.scanlines {
  background: linear-gradient(rgba(18, 16, 16, 0) 50%, rgba(0, 0, 0, 0.35) 50%);
  background-size: 100% 4px;
  pointer-events: none;
  position: absolute;
  inset: 0;
  z-index: 40;
}

.game-window {
  box-shadow:
    inset 0 0 80px rgba(0, 0, 0, 0.75),
    0 0 20px rgba(235, 87, 147, 0.2);
}

/* Viewport Layout */
.game-viewport {
  position: relative;
  width: 100%;
  flex: 1;
  overflow: hidden;
}

.stage-canvas {
  position: absolute;
  inset: 0;
  width: 100%;
  height: 100%;
  z-index: 0;
}

.scene-overlay {
  position: absolute;
  inset: 0;
  z-index: 10;
  pointer-events: none;
}

/* Boat Animations & Positioning */
.boat-wrapper {
  position: absolute;
  left: 50%;
  bottom: 14%;
  transform: translateX(-50%);
  width: 90%;
  max-width: 850px;
  height: auto;
  transition: transform 0.3s;
  pointer-events: auto;
}

.boat-svg {
  width: 100%;
  height: 100%;
  filter: drop-shadow(0 25px 25px rgb(0 0 0 / 0.5));
}

@keyframes boatBobbing {
  0% {
    transform: translateY(0px) rotate(0deg);
  }
  25% {
    transform: translateY(-4px) rotate(1.2deg);
  }
  50% {
    transform: translateY(2px) rotate(-0.8deg);
  }
  75% {
    transform: translateY(-7px) rotate(2deg);
  }
  100% {
    transform: translateY(0px) rotate(0deg);
  }
}

@keyframes boatTurbo {
  0% {
    transform: translateY(-2px) rotate(4deg) translateX(0px);
  }
  20% {
    transform: translateY(4px) rotate(2.5deg) translateX(-3px);
  }
  40% {
    transform: translateY(-6px) rotate(5deg) translateX(2px);
  }
  60% {
    transform: translateY(3px) rotate(3deg) translateX(-1px);
  }
  80% {
    transform: translateY(-8px) rotate(5.5deg) translateX(3px);
  }
  100% {
    transform: translateY(-2px) rotate(4deg) translateX(0px);
  }
}

.boat-normal {
  animation: boatBobbing 1.8s infinite ease-in-out;
}

.boat-fast {
  animation: boatTurbo 0.4s infinite ease-in-out;
}

/* Camera Shake */
@keyframes cameraShake {
  0% {
    transform: translate(1px, 1px) rotate(0deg);
  }
  10% {
    transform: translate(-1px, -2px) rotate(-0.5deg);
  }
  20% {
    transform: translate(-3px, 0px) rotate(0.5deg);
  }
  30% {
    transform: translate(3px, 2px) rotate(0deg);
  }
  40% {
    transform: translate(1px, -1px) rotate(0.5deg);
  }
  50% {
    transform: translate(-1px, 2px) rotate(-0.5deg);
  }
  60% {
    transform: translate(-3px, 1px) rotate(0deg);
  }
  70% {
    transform: translate(3px, 1px) rotate(-0.5deg);
  }
  80% {
    transform: translate(-1px, -1px) rotate(0.5deg);
  }
  90% {
    transform: translate(1px, 2px) rotate(0deg);
  }
  100% {
    transform: translate(1px, -2px) rotate(-0.5deg);
  }
}

.shaking {
  animation: cameraShake 0.15s infinite;
}

/* Nitro and Interactive Objects */
.nitro-group {
  transition: opacity 0.2s;
}

.flames-hidden {
  opacity: 0;
}

.interactive-object {
  cursor: pointer;
}

.interactive-object:hover {
  filter: drop-shadow(0 0 6px rgba(255, 230, 0, 0.75));
}

.nitro-active {
  filter: drop-shadow(0 0 15px #ff2a8d) brightness(1.15);
}

/* HUD Overlay Elements */
.hud-location {
  position: absolute;
  top: 1rem;
  left: 1rem;
  z-index: 20;
  display: flex;
  gap: 0.5rem;
}

.hud-location-box {
  background: rgba(0, 0, 0, 0.6);
  backdrop-filter: blur(4px);
  border: 1px solid rgba(6, 182, 212, 0.4);
  border-radius: 0.25rem;
  padding: 0.375rem 0.75rem;
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

@keyframes ping {
  75%,
  100% {
    transform: scale(2);
    opacity: 0;
  }
}

.ping-dot {
  width: 0.625rem;
  height: 0.625rem;
  border-radius: 9999px;
  background-color: #22d3ee;
  animation: ping 1s cubic-bezier(0, 0, 0.2, 1) infinite;
}

.location-text {
  color: #67e8f9;
  font-family: "Press Start 2P", cursive;
  font-size: 0.75rem;
  letter-spacing: 0.05em;
}

/* Dialog Overlay */
.dialog-container {
  position: absolute;
  top: 3rem;
  left: 50%;
  transform: translateX(-50%);
  z-index: 30;
  transition: opacity 0.3s;
  pointer-events: none;
  text-align: center;
  max-width: 32rem;
  width: 100%;
  padding: 0 1rem;
}

.dialog-hidden {
  opacity: 0;
}

.dialog-box {
  font-family: "Press Start 2P", cursive;
  color: #fde047;
  font-size: 0.875rem;
  text-shadow: 0 3px 0 #000000;
  background: rgba(0, 0, 0, 0.8);
  border: 2px solid rgba(234, 179, 8, 0.8);
  padding: 0.5rem 1rem;
  border-radius: 0.25rem;
  display: inline-block;
}

/* Speedometer HUD */
.speedometer {
  position: absolute;
  top: 1rem;
  right: 1rem;
  z-index: 20;
  background: rgba(15, 23, 42, 0.8);
  border: 2px solid #334155;
  border-radius: 0.5rem;
  padding: 0.5rem;
  display: flex;
  align-items: center;
  gap: 0.75rem;
}

.speed-text-container {
  text-align: right;
}

.speed-label {
  font-size: 10px;
  font-family: "Press Start 2P", cursive;
  color: #94a3b8;
}

.speed-value {
  font-family: "Press Start 2P", cursive;
  font-size: 1.125rem;
  color: #34d399;
}

.speedometer-gauge {
  width: 3rem;
  height: 3rem;
  border-radius: 9999px;
  border: 2px solid #475569;
  background: #020617;
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
}

.needle {
  width: 0.25rem;
  height: 1.25rem;
  background: #ef4444;
  position: absolute;
  bottom: 50%;
  transform-origin: bottom;
  transition: transform 0.3s;
}

.gauge-center {
  width: 0.5rem;
  height: 0.5rem;
  border-radius: 9999px;
  background: #94a3b8;
  z-index: 10;
}

/* SCUMM UI Panel */
.scumm-panel {
  height: 9rem;
  background: #1c1a24;
  border-top: 4px solid #4d4661;
  padding: 0.5rem;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  z-index: 20;
  position: relative;
  user-select: none;
}

.action-bar {
  width: 100%;
  background: rgba(0, 0, 0, 0.8);
  border: 1px solid #334155;
  padding: 0.25rem 0.75rem;
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-radius: 0.25rem;
}

.action-text {
  font-family: "Press Start 2P", cursive;
  color: #22d3ee;
  font-size: 0.75rem;
  letter-spacing: 0.05em;
}

.audio-btn {
  font-size: 0.75rem;
  font-family: "Press Start 2P", cursive;
  padding: 0.125rem 0.5rem;
  border-radius: 0.25rem;
  border: 1px solid rgba(245, 158, 11, 0.5);
  cursor: pointer;
  color: #fbbf24;
}

.audio-on {
  background-color: #064e3b;
}

.audio-off {
  background-color: #1e293b;
}
.audio-off:hover {
  background-color: #334155;
}

/* SCUMM Command Grid */
.scumm-controls-grid {
  display: grid;
  grid-template-columns: repeat(12, minmax(0, 1fr));
  gap: 0.5rem;
  flex: 1;
  margin-top: 0.375rem;
}

.verbs-grid {
  grid-column: span 5;
  display: grid;
  grid-template-columns: repeat(3, minmax(0, 1fr));
  gap: 0.25rem;
  font-size: 10px;
  font-family: "Press Start 2P", cursive;
}

.scumm-btn {
  background-color: #2a2538;
  border: 2px solid #5a5175;
  box-shadow:
    inset -2px -2px 0px #121019,
    inset 2px 2px 0px #7b709c;
  color: #55e6c1;
  transition: all 0.05s ease;
  cursor: pointer;
  padding: 0.25rem;
  text-align: center;
  border-radius: 0.25rem;
}

.scumm-btn:hover {
  color: #ffda79;
  background-color: #38314a;
  border-color: #7b709c;
}

.scumm-btn:active,
.scumm-btn.active {
  background-color: #1a1624;
  box-shadow:
    inset 2px 2px 0px #09080e,
    inset -2px -2px 0px #423b56;
  color: #ff5252;
}

.engine-controls {
  grid-column: span 7;
  background: rgba(15, 23, 42, 0.9);
  border: 2px solid #334155;
  border-radius: 0.25rem;
  padding: 0.5rem;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.engine-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.engine-title {
  font-family: "Press Start 2P", cursive;
  font-size: 0.75rem;
  color: #f472b6;
}

.mode-badge {
  font-family: "Press Start 2P", cursive;
  font-size: 10px;
  padding: 0.125rem 0.5rem;
  border-radius: 0.25rem;
  border: 1px solid;
}

.badge-cruise {
  background-color: #083344;
  color: #67e8f9;
  border-color: #06b6d4;
}

.badge-turbo {
  background-color: #500724;
  color: #f472b6;
  border-color: #ec4899;
  animation: pulse 2s cubic-bezier(0.4, 0, 0.6, 1) infinite;
}

@keyframes pulse {
  0%,
  100% {
    opacity: 1;
  }
  50% {
    opacity: 0.5;
  }
}

.action-buttons-row {
  display: grid;
  grid-template-columns: repeat(3, minmax(0, 1fr));
  gap: 0.5rem;
  margin: 0.25rem 0;
}

.engine-btn {
  border-radius: 0.25rem;
  padding: 0.5rem;
  font-family: "Press Start 2P", cursive;
  font-size: 0.75rem;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 0.25rem;
}

.turbo-action-btn {
  color: #f472b6;
  border-color: rgba(236, 72, 153, 0.5);
}

.talk-action-btn {
  color: #fde047;
  border-color: rgba(234, 179, 8, 0.5);
}

.status-footer {
  font-size: 11px;
  font-family: "VT323", monospace;
  color: #94a3b8;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.scumm-version {
  color: #22d3ee;
}

.boat-title-text {
  font-family: "Press Start 2P", cursive;
  font-size: 12px;
  fill: #ffffff;
  font-weight: bold;
}

/* Footer Instructions */
.footer-info {
  margin-top: 0.5rem;
  text-align: center;
  font-size: 0.75rem;
  font-family: "VT323", monospace;
  color: #94a3b8;
  display: flex;
  align-items: center;
  gap: 1rem;
}
</style>
