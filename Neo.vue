<template>
  <div class="matrix-scene-wrapper">
    <!-- Main Game Frame Screen (Rendered in retro 320x180 resolution scaled via SVG Grid) -->
    <main class="game-main">
 

      <div
        class="game-viewport"
        :class="{ crt: crtEnabled, 'crt-curved': crtEnabled }"
      >
        <!-- Matrix Rain Canvas Background Overlay -->
        <canvas ref="matrixCanvasRef" class="matrix-canvas"></canvas>

        <!-- 16-BIT SVG PIXEL ART CANVAS (Canvas Native aspect 320x180) -->
        <svg
          class="pixel-scene"
          viewBox="0 0 320 180"
          xmlns="http://www.w3.org/2000/svg"
        >
          <defs>
            <!-- Pixel patterns & Grid shaders -->
            <pattern
              id="pixelGrid"
              width="1"
              height="1"
              patternUnits="userSpaceOnUse"
            >
              <rect width="1" height="1" fill="none" />
            </pattern>

            <!-- Dithering Pattern for 90s PC Shadows -->
            <pattern
              id="ditherDark"
              width="2"
              height="2"
              patternUnits="userSpaceOnUse"
            >
              <rect x="0" y="0" width="1" height="1" fill="#000" opacity="0.4" />
              <rect x="1" y="1" width="1" height="1" fill="#000" opacity="0.4" />
            </pattern>
            <pattern
              id="ditherFire"
              width="2"
              height="2"
              patternUnits="userSpaceOnUse"
            >
              <rect
                x="1"
                y="0"
                width="1"
                height="1"
                fill="#ff5500"
                opacity="0.35"
              />
              <rect
                x="0"
                y="1"
                width="1"
                height="1"
                fill="#aa2200"
                opacity="0.35"
              />
            </pattern>
          </defs>

          <!-- ================= ROOM BACKGROUND & WALLS ================= -->
          <!-- Dark Room Base Wall -->
          <rect x="0" y="0" width="320" height="140" fill="#070d08" />
          <!-- Dithered wallpaper texture -->
          <rect x="0" y="0" width="320" height="140" fill="url(#ditherDark)" />

          <!-- Gothic Arched Windows Left & Right -->
          <!-- Left Window -->
          <g transform="translate(15, 20)">
            <rect x="0" y="10" width="30" height="70" fill="#03080d" />
            <path d="M 0,10 Q 15,-10 30,10" fill="#03080d" />
            <rect x="14" y="-2" width="2" height="82" fill="#0c1d1a" />
            <rect x="0" y="35" width="30" height="2" fill="#0c1d1a" />
            <rect x="0" y="60" width="30" height="2" fill="#0c1d1a" />
            <!-- Raindrops (Pixelated) -->
            <rect x="6" y="15" width="1" height="4" fill="#1e4e40" />
            <rect x="22" y="25" width="1" height="5" fill="#1e4e40" />
            <rect x="10" y="48" width="1" height="3" fill="#1e4e40" />
          </g>
          <!-- Right Window -->
          <g transform="translate(275, 20)">
            <rect x="0" y="10" width="30" height="70" fill="#03080d" />
            <path d="M 0,10 Q 15,-10 30,10" fill="#03080d" />
            <rect x="14" y="-2" width="2" height="82" fill="#0c1d1a" />
            <rect x="0" y="35" width="30" height="2" fill="#0c1d1a" />
            <rect x="0" y="60" width="30" height="2" fill="#0c1d1a" />
            <!-- Raindrops -->
            <rect x="8" y="20" width="1" height="4" fill="#1e4e40" />
            <rect x="18" y="40" width="1" height="6" fill="#1e4e40" />
          </g>

          <!-- Wooden Floor (Perspective Plank Grid) -->
          <rect x="0" y="140" width="320" height="40" fill="#100b07" />
          <path
            d="M 0,140 L 320,140 M 0,150 L 320,150 M 0,165 L 320,165 M 0,180 L 320,180"
            stroke="#080503"
            stroke-width="1"
          />
          <path
            d="M 40,140 L 10,180 M 100,140 L 70,180 M 160,140 L 160,180 M 220,140 L 250,180 M 280,140 L 310,180"
            stroke="#080503"
            stroke-width="1"
          />

          <!-- ================= PIXEL FIREPLACE STRUCTURE ================= -->
          <g transform="translate(125, 60)">
            <!-- Outer Stone Chimney -->
            <rect x="0" y="0" width="70" height="80" fill="#141815" />
            <rect x="-4" y="0" width="78" height="8" fill="#1f2621" />
            <rect x="2" y="8" width="66" height="4" fill="#0b0e0c" />

            <!-- Fire Brick Opening -->
            <rect x="12" y="30" width="46" height="50" fill="#080503" />
            <path d="M 12,40 Q 35,25 58,40" fill="#080503" />

            <!-- Brick Details -->
            <rect x="16" y="42" width="8" height="4" fill="#1c0f0a" />
            <rect x="28" y="42" width="8" height="4" fill="#1c0f0a" />
            <rect x="44" y="42" width="8" height="4" fill="#1c0f0a" />
            <rect x="20" y="52" width="8" height="4" fill="#1c0f0a" />
            <rect x="36" y="52" width="8" height="4" fill="#1c0f0a" />

            <!-- Cast Iron Grate -->
            <rect x="16" y="70" width="38" height="2" fill="#2b2b2b" />
            <rect x="20" y="65" width="2" height="7" fill="#111" />
            <rect x="28" y="65" width="2" height="7" fill="#111" />
            <rect x="36" y="65" width="2" height="7" fill="#111" />
            <rect x="44" y="65" width="2" height="7" fill="#111" />

            <!-- Burning Wood Logs -->
            <rect x="20" y="71" width="30" height="4" fill="#2d160a" />
            <rect x="24" y="68" width="22" height="4" fill="#3d1e0e" />

            <!-- PIXELATED FIRE ANIMATION -->
            <g class="pixel-fire-glow">
              <!-- Fire Base Glow -->
              <rect x="20" y="66" width="30" height="6" fill="#ff2200" />
              <rect x="24" y="62" width="22" height="6" fill="#ff6600" />
              <rect x="28" y="56" width="14" height="8" fill="#ffcc00" />
              <rect x="32" y="52" width="6" height="6" fill="#ffffff" />

              <!-- Dynamic Fire Sparks Pixels -->
              <rect x="22" y="54" width="2" height="4" fill="#ff4400" />
              <rect x="44" y="52" width="2" height="4" fill="#ff6600" />
              <rect x="26" y="46" width="2" height="3" fill="#ffaa00" />
              <rect x="38" y="44" width="2" height="4" fill="#ffdd00" />
            </g>
          </g>

          <!-- Fire Ambient Light Reflection Overlay -->
          <rect
            x="80"
            y="80"
            width="160"
            height="100"
            fill="url(#ditherFire)"
            class="pixel-fire-glow"
            pointer-events="none"
          />

          <!-- ================= LEFT ARMCHAIR & NEO (PIXEL SILHOUETTE) ================= -->
          <g transform="translate(35, 75)">
            <!-- Chair Back (Green 16-Bit Chesterfield) -->
            <rect x="5" y="10" width="45" height="65" fill="#0d2b1a" />
            <rect x="10" y="5" width="35" height="10" fill="#143d26" />
            <!-- Tufting Pixel Dots -->
            <rect x="15" y="20" width="2" height="2" fill="#05140b" />
            <rect x="33" y="20" width="2" height="2" fill="#05140b" />
            <rect x="24" y="32" width="2" height="2" fill="#05140b" />
            <rect x="15" y="44" width="2" height="2" fill="#05140b" />
            <rect x="33" y="44" width="2" height="2" fill="#05140b" />

            <!-- Left Armrest -->
            <rect x="0" y="40" width="12" height="35" fill="#143d26" />
            <rect x="0" y="38" width="14" height="4" fill="#1e5435" />

            <!-- NEO PIXEL CHARACTER -->
            <!-- Legs & Trenchcoat lower -->
            <rect x="20" y="45" width="22" height="30" fill="#0a0f0c" />
            <!-- Torso / Dark Shirt -->
            <rect x="20" y="28" width="18" height="20" fill="#050806" />
            <!-- Fire Light Rim on Neo's Back/Side -->
            <rect x="36" y="28" width="2" height="18" fill="#d97706" />
            <!-- Neck -->
            <rect x="27" y="22" width="5" height="6" fill="#8c5a3c" />
            <!-- Head angled toward Morpheus -->
            <rect x="24" y="12" width="11" height="11" fill="#a36e4c" />
            <!-- Hair -->
            <rect x="23" y="10" width="12" height="4" fill="#0a0a0a" />
            <rect x="22" y="12" width="4" height="6" fill="#0a0a0a" />

            <!-- Right Armrest (Foreground) -->
            <rect x="38" y="42" width="14" height="35" fill="#1a4a2e" />
            <rect x="36" y="38" width="16" height="4" fill="#23663f" />
            <!-- Chair Base Legs -->
            <rect x="8" y="73" width="6" height="6" fill="#050a07" />
            <rect x="38" y="73" width="6" height="6" fill="#050a07" />
          </g>

          <!-- ================= RIGHT ARMCHAIR & MORPHEUS (PIXEL ART) ================= -->
          <g transform="translate(230, 75)">
            <!-- Chair Back -->
            <rect x="5" y="10" width="45" height="65" fill="#0d2b1a" />
            <rect x="10" y="5" width="35" height="10" fill="#143d26" />
            <!-- Tufting Pixel Dots -->
            <rect x="15" y="20" width="2" height="2" fill="#05140b" />
            <rect x="33" y="20" width="2" height="2" fill="#05140b" />
            <rect x="24" y="32" width="2" height="2" fill="#05140b" />

            <!-- Left Armrest (Fire facing side) -->
            <rect x="0" y="42" width="14" height="35" fill="#1a4a2e" />
            <rect
              x="-2"
              y="38"
              width="16"
              height="4"
              fill="#ff7700"
              opacity="0.8"
            />

            <!-- MORPHEUS PIXEL CHARACTER -->
            <!-- Leather Trench Coat -->
            <rect x="12" y="28" width="30" height="45" fill="#181310" />
            <rect x="18" y="30" width="18" height="20" fill="#2d221c" />
            <!-- Purple Tie -->
            <rect x="26" y="34" width="3" height="10" fill="#581c87" />

            <!-- Neck -->
            <rect x="25" y="22" width="6" height="6" fill="#6b3f23" />
            <!-- Bald Head -->
            <rect x="22" y="11" width="12" height="12" fill="#7c4a2a" />
            <!-- Head Highlight -->
            <rect x="24" y="11" width="6" height="2" fill="#9e633b" />
            <rect x="21" y="15" width="2" height="6" fill="#ff7700" />

            <!-- Extended Hands & Pills -->
            <!-- Left Hand (Red Pill - Screen Left for Morpheus) -->
            <g transform="translate(-10, 36)">
              <rect x="5" y="6" width="10" height="4" fill="#181310" />
              <rect x="0" y="6" width="6" height="5" fill="#7c4a2a" />
              <!-- RED PILL PIXEL -->
              <g class="cursor-pointer" @click="selectPill('red')">
                <rect x="1" y="3" width="4" height="2" fill="#ef4444" />
                <rect x="2" y="2" width="2" height="1" fill="#fca5a5" />
              </g>
            </g>

            <!-- Right Hand (Blue Pill - Screen Right) -->
            <g transform="translate(38, 36)">
              <rect x="-5" y="6" width="10" height="4" fill="#181310" />
              <rect x="3" y="6" width="6" height="5" fill="#7c4a2a" />
              <!-- BLUE PILL PIXEL -->
              <g class="cursor-pointer" @click="selectPill('blue')">
                <rect x="4" y="3" width="4" height="2" fill="#3b82f6" />
                <rect x="5" y="2" width="2" height="1" fill="#bfdbfe" />
              </g>
            </g>

            <!-- SUNGLASSES WITH PIXEL REFLECTIONS -->
            <g
              class="cursor-pointer"
              transform="translate(23, 15)"
              @click="zoomGlasses"
            >
              <!-- Frame Bridge -->
              <rect x="4" y="1" width="3" height="1" fill="#d97706" />
              <!-- Left Lens -->
              <rect x="0" y="1" width="4" height="3" fill="#000" />
              <rect x="1" y="2" width="1" height="1" fill="#ef4444" />
              <!-- Right Lens -->
              <rect x="7" y="1" width="4" height="3" fill="#000" />
              <rect x="9" y="2" width="1" height="1" fill="#3b82f6" />
            </g>

            <!-- Right Armrest -->
            <rect x="42" y="40" width="12" height="35" fill="#143d26" />
            <!-- Base Legs -->
            <rect x="8" y="73" width="6" height="6" fill="#050a07" />
            <rect x="38" y="73" width="6" height="6" fill="#050a07" />
          </g>

          <!-- Mouse Cursor Icon (SCUMM Point & Click Crosshair) -->
          <g transform="translate(160, 110)">
            <rect x="-3" y="0" width="7" height="1" fill="#4ade80" />
            <rect x="0" y="-3" width="1" height="7" fill="#4ade80" />
          </g>
        </svg>

        <!-- Retro Sunglasses Inspection Modal Overlay -->
        <div
          class="glasses-modal-overlay"
          :class="{ active: isGlassesModalOpen }"
        >
          <button class="close-btn" @click="closeGlassesModal">
            [ X CLOSE ]
          </button>

          <div class="glasses-title">16-BIT GLASSES REFLECTION</div>

          <!-- 8-Bit Pixel Close Up SVG -->
          <svg viewBox="0 0 160 70" class="glasses-svg-container">
            <!-- Glasses Golden Frame -->
            <rect x="74" y="20" width="12" height="4" fill="#d97706" />

            <!-- Left Lens (Red Pill Side) -->
            <g class="cursor-pointer" @click="selectPill('red')">
              <rect
                x="10"
                y="10"
                width="60"
                height="45"
                fill="#111"
                stroke="#d97706"
                stroke-width="3"
              />
              <rect x="14" y="14" width="52" height="37" fill="#1a0803" />
              <!-- Room & Fire Dither Reflection -->
              <rect x="14" y="35" width="52" height="16" fill="#9a3412" />
              <!-- Red Pill Large Pixel -->
              <rect x="25" y="28" width="14" height="8" fill="#ef4444" />
              <rect x="25" y="28" width="7" height="8" fill="#fca5a5" />
              <!-- Neo Silhouette -->
              <rect x="48" y="20" width="10" height="25" fill="#000" />
              <text x="18" y="62" fill="#ef4444" font-size="6">RED PILL</text>
            </g>

            <!-- Right Lens (Blue Pill Side) -->
            <g class="cursor-pointer" @click="selectPill('blue')">
              <rect
                x="90"
                y="10"
                width="60"
                height="45"
                fill="#111"
                stroke="#d97706"
                stroke-width="3"
              />
              <rect x="94" y="14" width="52" height="37" fill="#031024" />
              <!-- Room Reflection -->
              <rect x="94" y="35" width="52" height="16" fill="#1e40af" />
              <!-- Blue Pill Large Pixel -->
              <rect x="120" y="28" width="14" height="8" fill="#3b82f6" />
              <rect x="120" y="28" width="7" height="8" fill="#bfdbfe" />
              <!-- Neo Silhouette -->
              <rect x="102" y="20" width="10" height="25" fill="#000" />
              <text x="96" y="62" fill="#60a5fa" font-size="6">BLUE PILL</text>
            </g>
          </svg>
        </div>
      </div>

      <!-- SCUMM / LucasArts 90s Adventure UI Bar -->
      <div class="scumm-bar">
        <!-- Dialogue Prompt Box -->
        <div class="dialogue-box">
          <span class="morpheus-speaker">MORPHEUS:</span>
          <span class="cursor-blink">{{ currentDialogue }}</span>
        </div>
      </div>
    </main>

    <!-- Retro Game Choice Outcome Screen Modal -->
    <div class="choice-modal-overlay" :class="{ active: isChoiceModalOpen }">
      <div class="choice-card-box" :class="choiceBoxClass">
        <h2 class="choice-title" :class="choiceTitleClass">
          {{ choiceTitle }}
        </h2>
        <p class="choice-description">{{ choiceDescription }}</p>
        <button class="restart-btn" @click="resetChoice">
          [ RESTART CONSTRUCT ]
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue';

// --- State ---
const crtEnabled = ref(true);
const isAudioOn = ref(false);
const isGlassesModalOpen = ref(false);
const isChoiceModalOpen = ref(false);

const choiceTitle = ref('');
const choiceDescription = ref('');
const choiceTitleClass = ref('');
const choiceBoxClass = ref('');

const currentDialogue = ref(
  '"Let me tell you why you are here."'
);

const quotes = [
  '"Let me tell you why you are here."',
  '"You are here because you know something."',
  '"What you know you can\'t explain, but you feel it."',
  '"You\'ve felt it your entire life."',
  '"That there\'s something wrong with the world, you don\'t know what it is, but it\'s there."',
  '"Like a splinter in your mind, driving you mad."',
  '"It is this feeling that has brought you to me."',
  '"Do you know what I\'m talking about?"',
  '"Take the blue pill, the story ends. Take the red pill, stay in Wonderland."',
  '"I can only show you the door. You have to walk through it."',
  '"Have you ever had a dream that felt completely real?"',
];

// --- Canvas Matrix Rain ---
const matrixCanvasRef = ref(null);
let matrixInterval = null;

function initMatrixRain() {
  const canvas = matrixCanvasRef.value;
  if (!canvas) return;
  const ctx = canvas.getContext('2d');

  canvas.width = 320;
  canvas.height = 180;

  const chars = '0123456789ABCDEF';
  const fontSize = 8;
  const columns = Math.floor(canvas.width / fontSize);
  const drops = Array(columns).fill(1);

  function drawMatrix() {
    ctx.fillStyle = 'rgba(0, 0, 0, 0.15)';
    ctx.fillRect(0, 0, canvas.width, canvas.height);
    ctx.fillStyle = '#22c55e';
    ctx.font = fontSize + 'px monospace';

    for (let i = 0; i < drops.length; i++) {
      const text = chars.charAt(Math.floor(Math.random() * chars.length));
      ctx.fillText(text, i * fontSize, drops[i] * fontSize);
      if (drops[i] * fontSize > canvas.height && Math.random() > 0.95) {
        drops[i] = 0;
      }
      drops[i]++;
    }
  }

  matrixInterval = setInterval(drawMatrix, 60);
}

// --- Audio Synth Logic ---
let audioCtx = null;
let fireNoiseNode = null;
let arpInterval = null;

function initChiptuneAudio() {
  if (audioCtx) return;
  audioCtx = new (window.AudioContext || window.webkitAudioContext)();

  const bufferSize = audioCtx.sampleRate * 1;
  const noiseBuffer = audioCtx.createBuffer(
    1,
    bufferSize,
    audioCtx.sampleRate
  );
  const data = noiseBuffer.getChannelData(0);
  for (let i = 0; i < bufferSize; i++) {
    data[i] = Math.random() * 2 - 1;
  }

  fireNoiseNode = audioCtx.createBufferSource();
  fireNoiseNode.buffer = noiseBuffer;
  fireNoiseNode.loop = true;

  const filter = audioCtx.createBiquadFilter();
  filter.type = 'bandpass';
  filter.frequency.value = 600;
  filter.Q.value = 3;

  const gain = audioCtx.createGain();
  gain.gain.value = 0.06;

  fireNoiseNode.connect(filter);
  filter.connect(gain);
  gain.connect(audioCtx.destination);

  fireNoiseNode.start();
  startChiptuneArp();
}

function startChiptuneArp() {
  const notes = [110, 130.81, 146.83, 164.81]; // Am synth chords
  let step = 0;
  arpInterval = setInterval(() => {
    if (!isAudioOn.value || !audioCtx) return;
    const osc = audioCtx.createOscillator();
    const gain = audioCtx.createGain();
    osc.type = 'square';
    osc.frequency.setValueAtTime(
      notes[step % notes.length],
      audioCtx.currentTime
    );

    gain.gain.setValueAtTime(0.015, audioCtx.currentTime);
    gain.gain.exponentialRampToValueAtTime(
      0.001,
      audioCtx.currentTime + 0.15
    );

    osc.connect(gain);
    gain.connect(audioCtx.destination);

    osc.start();
    osc.stop(audioCtx.currentTime + 0.15);
    step++;
  }, 200);
}

function playBeep(freq, duration) {
  if (!audioCtx || audioCtx.state !== 'running') return;
  const osc = audioCtx.createOscillator();
  const gain = audioCtx.createGain();
  osc.type = 'triangle';
  osc.frequency.value = freq;
  gain.gain.setValueAtTime(0.1, audioCtx.currentTime);
  gain.gain.exponentialRampToValueAtTime(
    0.001,
    audioCtx.currentTime + duration
  );
  osc.connect(gain);
  gain.connect(audioCtx.destination);
  osc.start();
  osc.stop(audioCtx.currentTime + duration);
}

function toggleAudio() {
  if (!isAudioOn.value) {
    initChiptuneAudio();
    if (audioCtx && audioCtx.state === 'suspended') {
      audioCtx.resume();
    }
    isAudioOn.value = true;
    playBeep(800, 0.1);
  } else {
    if (audioCtx) {
      audioCtx.suspend();
    }
    isAudioOn.value = false;
  }
}

function toggleCRT() {
  crtEnabled.value = !crtEnabled.value;
  playBeep(600, 0.05);
}

// --- Modal Controls ---
function zoomGlasses() {
  playBeep(523, 0.08);
  isGlassesModalOpen.value = true;
}

function closeGlassesModal() {
  playBeep(300, 0.08);
  isGlassesModalOpen.value = false;
}

function selectPill(color) {
  closeGlassesModal();
  isChoiceModalOpen.value = true;

  if (color === 'red') {
    playBeep(150, 0.4);
    choiceTitle.value = 'RED PILL SELECTED';
    choiceTitleClass.value = 'red-text';
    choiceDescription.value =
      'SYSTEM WAKEUP SEQUENCE INITIATED... You disconnect from the Matrix 1.0 construct. Welcome to 1999.';
    choiceBoxClass.value = 'red-border';
  } else {
    playBeep(600, 0.4);
    choiceTitle.value = 'BLUE PILL SELECTED';
    choiceTitleClass.value = 'blue-text';
    choiceDescription.value =
      'MEMORY PURGED... You wake up in your apartment believing whatever you want to believe. Game Over.';
    choiceBoxClass.value = 'blue-border';
  }
}

function resetChoice() {
  playBeep(400, 0.08);
  isChoiceModalOpen.value = false;
}

// --- Dialogue Cycler ---
let quoteInterval = null;
function startDialogueCycle() {
  let qIdx = 0;
  quoteInterval = setInterval(() => {
    qIdx = (qIdx + 1) % quotes.length;
    currentDialogue.value = quotes[qIdx];
  }, 7000);
}

// --- Lifecycle ---
onMounted(() => {
  initMatrixRain();
  startDialogueCycle();
});

onUnmounted(() => {
  if (matrixInterval) clearInterval(matrixInterval);
  if (quoteInterval) clearInterval(quoteInterval);
  if (arpInterval) clearInterval(arpInterval);
  if (audioCtx) {
    audioCtx.close();
  }
});
</script>

<style scoped>
 
/* Base Page Styles */
.matrix-scene-wrapper {
  background-color: #030503;
  color: #4ade80;
  font-family: 'Press Start 2P', monospace;
  image-rendering: pixelated;
  image-rendering: crisp-edges;
  user-select: none;
  min-height: 100vh;
  padding: 16px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: center;
  position: relative;
  box-sizing: border-box;
}

/* Pixel art crisp rendering rules */
canvas,
svg,
img {
  image-rendering: pixelated;
  image-rendering: crisp-edges;
  display: block;
}

/* Top Header Navigation */
.retro-header {
  width: 100%;
  max-width: 896px;
  z-index: 30;
  margin-bottom: 12px;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  align-items: center;
  gap: 8px;
  background-color: #000;
  border: 4px solid #166534;
  padding: 12px;
  box-shadow: 0 0 15px rgba(0, 255, 0, 0.2);
}

.header-title-container {
  display: flex;
  align-items: center;
  gap: 8px;
}

.pulse-dot {
  display: inline-block;
  width: 12px;
  height: 12px;
  background-color: #22c55e;
  animation: pulse 1.5s cubic-bezier(0.4, 0, 0.6, 1) infinite;
}

@keyframes pulse {
  0%,
  100% {
    opacity: 1;
  }
  50% {
    opacity: 0.3;
  }
}

.header-title {
  font-size: 12px;
  letter-spacing: 0.05em;
  color: #86efac;
}

.header-buttons {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
  font-size: 10px;
}

.btn-green-ui {
  padding: 4px 8px;
  background-color: #052e16;
  border: 2px solid #16a34a;
  color: #bbf7d0;
  cursor: pointer;
  font-family: inherit;
  font-size: 10px;
}

.btn-green-ui:hover {
  background-color: #166534;
}

.btn-amber-ui {
  padding: 4px 8px;
  background-color: #451a03;
  border: 2px solid #d97706;
  color: #fde68a;
  cursor: pointer;
  font-family: inherit;
  font-size: 10px;
}

.btn-amber-ui:hover {
  background-color: #92400e;
}

/* Main Game Layout Container */
.game-main {
  width: 100%;
  max-width: 1196px;
  position: relative;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.game-viewport {
  width: 100%;
  position: relative;
  border: 8px solid #171717;
  background-color: #000;
  overflow: hidden;
  box-shadow: 0 0 40px rgba(0, 100, 0, 0.4);
}

/* CRT Screen Curved Overlay & Scanlines */
.crt::before {
  content: ' ';
  display: block;
  position: absolute;
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
  background: linear-gradient(
      rgba(18, 16, 16, 0) 50%,
      rgba(0, 0, 0, 0.4) 50%
    ),
    linear-gradient(
      90deg,
      rgba(255, 0, 0, 0.04),
      rgba(0, 255, 0, 0.02),
      rgba(0, 0, 255, 0.04)
    );
  z-index: 40;
  background-size: 100% 4px, 6px 100%;
  pointer-events: none;
}

.crt::after {
  content: ' ';
  display: block;
  position: absolute;
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
  background: rgba(18, 16, 16, 0.08);
  opacity: 0;
  pointer-events: none;
  z-index: 40;
}

.crt-curved {
  box-shadow: inset 0 0 80px rgba(0, 0, 0, 0.85);
  border-radius: 18px;
}

/* Canvas Matrix Rain */
.matrix-canvas {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  width: 100%;
  height: 100%;
  z-index: 10;
  opacity: 0.2;
  pointer-events: none;
}

.pixel-scene {
  width: 100%;
  height: auto;
  position: relative;
  z-index: 0;
}

/* Flicker for Pixel Fire Light */
@keyframes pixelFlicker {
  0%,
  100% {
    opacity: 0.9;
  }
  25% {
    opacity: 0.7;
  }
  50% {
    opacity: 1;
  }
  75% {
    opacity: 0.8;
  }
}

.pixel-fire-glow {
  animation: pixelFlicker 0.25s steps(2) infinite;
}

.cursor-pointer {
  cursor: pointer;
}

/* Glasses Inspector Modal Overlay */
.glasses-modal-overlay {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.95);
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  opacity: 0;
  pointer-events: none;
  transition: opacity 0.3s ease;
  z-index: 50;
  padding: 16px;
}

.glasses-modal-overlay.active {
  opacity: 1;
  pointer-events: auto;
}

.close-btn {
  position: absolute;
  top: 8px;
  right: 8px;
  font-size: 10px;
  border: 2px solid #166534;
  background-color: #052e16;
  padding: 4px 8px;
  color: #86efac;
  cursor: pointer;
  font-family: inherit;
}

.close-btn:hover {
  background-color: #15803d;
}

.glasses-title {
  color: #4ade80;
  text-align: center;
  font-size: 10px;
  margin-bottom: 12px;
  letter-spacing: 0.1em;
  text-transform: uppercase;
}

.glasses-svg-container {
  width: 100%;
  max-width: 448px;
  height: auto;
  border: 4px solid #166534;
  background-color: #000;
}

/* SCUMM / LucasArts 90s Adventure UI Bar */
.scumm-bar {
  width: 100%;
  margin-top: 8px;
  background-color: #000;
  border: 4px solid #14532d;
  padding: 12px;
  display: flex;
  flex-direction: column;
  gap: 12px;
  box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.5);
}

.dialogue-box {
  width: 100%;
  min-height: 48px;
  background-color: #0a0a0a;
  border: 2px solid #166534;
  padding: 10px;
  font-size: 11px;
  color: #86efac;
  line-height: 1.6;
}

.morpheus-speaker {
  color: #fbbf24;
}

.cursor-blink::after {
  content: '█';
  animation: blink 0.8s steps(2) infinite;
}

@keyframes blink {
  0%,
  100% {
    opacity: 1;
  }
  50% {
    opacity: 0;
  }
}

/* Fullscreen Choice Outcome Overlay */
.choice-modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.95);
  z-index: 50;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  opacity: 0;
  pointer-events: none;
  transition: opacity 0.5s ease;
  padding: 16px;
  text-align: center;
}

.choice-modal-overlay.active {
  opacity: 1;
  pointer-events: auto;
}

.choice-card-box {
  max-width: 448px;
  width: 100%;
  padding: 24px;
  border: 4px solid #16a34a;
  background-color: #000;
  box-shadow: 0 0 30px rgba(0, 255, 0, 0.3);
}

.choice-card-box.red-border {
  border-color: #dc2626;
  box-shadow: 0 0 30px rgba(255, 0, 0, 0.5);
}

.choice-card-box.blue-border {
  border-color: #2563eb;
  box-shadow: 0 0 30px rgba(0, 100, 255, 0.5);
}

.choice-title {
  font-size: 16px;
  font-weight: bold;
  margin-bottom: 16px;
  letter-spacing: 0.05em;
}

.choice-title.red-text {
  color: #ef4444;
}
.choice-title.blue-text {
  color: #60a5fa;
}

.choice-description {
  font-size: 10px;
  color: #86efac;
  margin-bottom: 24px;
  line-height: 1.8;
}

.restart-btn {
  padding: 8px 16px;
  background-color: #052e16;
  border: 2px solid #22c55e;
  color: #bbf7d0;
  cursor: pointer;
  font-family: inherit;
  font-size: 10px;
  font-weight: bold;
}

.restart-btn:hover {
  background-color: #15803d;
}

@media (min-width: 768px) {
  .header-title {
    font-size: 14px;
  }
}
</style>