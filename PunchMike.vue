<template>
  <div class="fixed inset-0 w-screen h-screen bg-[#0d0614] text-white flex flex-col items-center justify-center p-2 sm:p-4 overflow-hidden select-none font-arcade">
    
    <!-- Outer Retro Cabinet Framing (Fullscreen Max Fill) -->
    <div class="relative w-full h-full max-w-7xl max-h-[96vh] bg-stone-900 border-4 border-yellow-500 rounded-xl p-3 sm:p-6 shadow-2xl flex flex-col justify-between">
        
        <!-- CRT Screen Overlay Toggle Container -->
        <div 
          id="crt-layer" 
          class="scanlines absolute inset-0 z-40 rounded-lg pointer-events-none transition-opacity duration-300"
          :class="{ 'opacity-0': !crtEnabled }"
        ></div>

        <!-- Top Arcade Bar / Header -->
        <div class="flex flex-wrap justify-between items-center bg-black border-2 border-stone-700 p-2 mb-2 text-xs sm:text-sm text-yellow-400 rounded shrink-0">
            <div class="flex items-center gap-2">
                <span class="inline-block w-3 h-3 bg-red-600 rounded-full animate-ping"></span>
                <span>INFINITE MATCH 1987 (FULLSCREEN)</span>
            </div>
            <div class="flex gap-4 items-center">
                <button @click="toggleScanlines" class="hover:text-white transition cursor-pointer">
                    CRT: <span :class="crtEnabled ? 'text-green-400' : 'text-red-400'">{{ crtEnabled ? 'ON' : 'OFF' }}</span>
                </button>
                <button @click="toggleSound" class="hover:text-white transition cursor-pointer">
                    SOUND: <span :class="soundEnabled ? 'text-green-400' : 'text-red-400'">{{ soundEnabled ? 'ON' : 'OFF' }}</span>
                </button>
                <button @click="toggleFullscreen" class="bg-yellow-500 text-black px-2 py-1 text-[10px] rounded font-bold hover:bg-yellow-400 transition cursor-pointer">
                    {{ isFullscreen ? 'EXIT FS' : 'FULLSCREEN' }}
                </button>
            </div>
        </div>

        <!-- Top HUD Dashboard -->
        <div class="bg-blue-950 border-4 border-white p-3 mb-2 rounded-lg flex flex-col sm:flex-row justify-between items-center text-xs gap-3 shrink-0">
            <!-- Little Mac Stats -->
            <div class="w-full sm:w-5/12">
                <div class="flex justify-between mb-1">
                    <span class="text-green-400 font-bold">LITTLE MAC</span>
                    <span class="text-yellow-300">STARS: <span id="star-count">★ {{ stars }}</span></span>
                </div>
                <div class="w-full bg-stone-800 h-5 border-2 border-white rounded relative overflow-hidden">
                    <div id="player-hp" class="bg-green-500 h-full transition-all duration-150" :style="{ width: playerHp + '%' }"></div>
                </div>
                <div class="flex justify-between text-[10px] mt-1 text-gray-300">
                    <span>STAMINA: <span class="text-red-400">♥ ∞</span></span>
                    <span>HITS: <span>{{ playerHits }}</span></span>
                </div>
            </div>

            <!-- Match Timer & Info -->
            <div class="text-center my-1 sm:my-0">
                <div class="text-red-500 font-bold text-sm sm:text-base">ROUND <span id="round-num">{{ round }}</span></div>
                <div class="text-white text-lg sm:text-xl font-extrabold my-0.5" id="timer">{{ formattedTimer }}</div>
                <div class="text-[9px] text-yellow-200 uppercase tracking-widest">{{ phaseIndicator }}</div>
            </div>

            <!-- Tyson Stats -->
            <div class="w-full sm:w-5/12 text-right">
                <div class="flex justify-between mb-1">
                    <span class="text-gray-300 text-[10px]">HITS: <span>{{ tysonHits }}</span></span>
                    <span class="text-red-500 font-bold">MIKE TYSON</span>
                </div>
                <div class="w-full bg-stone-800 h-5 border-2 border-white rounded relative overflow-hidden">
                    <div id="tyson-hp" class="bg-red-600 h-full transition-all duration-150" :style="{ width: tysonHp + '%' }"></div>
                </div>
                <div class="text-[10px] mt-1 text-yellow-400">
                    STATUS: <span class="text-white">{{ tysonStatus }}</span>
                </div>
            </div>
        </div>

        <!-- Main Boxing Ring Viewport (Flexible Grow Area) -->
        <div 
          id="game-viewport" 
          class="relative w-full flex-grow bg-slate-900 border-4 border-white rounded-lg overflow-hidden flex items-center justify-center my-2"
          :class="{ 'screen-shake': isShaking }"
        >
            <!-- Floating Hit/Dodge Text Layer -->
            <div id="text-fx-layer" class="absolute inset-0 z-30 pointer-events-none flex items-center justify-center">
              <div v-for="fx in floatingTexts" :key="fx.id" :class="['floating-text absolute font-extrabold text-sm sm:text-lg', fx.color]">
                {{ fx.text }}
              </div>
            </div>

            <!-- Main Interactive Vector SVG Scene -->
            <svg id="boxing-ring-svg" class="w-full h-full max-h-full" viewBox="0 0 800 600" preserveAspectRatio="xMidYMid meet">
                <defs>
                    <pattern id="crowd-pattern" width="20" height="20" patternUnits="userSpaceOnUse">
                        <rect width="10" height="10" fill="#2d1b4e" />
                        <rect x="10" width="10" height="10" fill="#1b1035" />
                        <rect y="10" width="10" height="10" fill="#3c2468" />
                        <rect x="10" y="10" width="10" height="10" fill="#150a2b" />
                        <circle cx="5" cy="5" r="2" fill="#e2b714" />
                        <circle cx="15" cy="15" r="2" fill="#ef4444" />
                        <circle cx="15" cy="5" r="2" fill="#3b82f6" />
                    </pattern>
                </defs>

                <!-- Arena Background & Audience -->
                <rect width="800" height="260" fill="url(#crowd-pattern)" />
                <rect y="240" width="800" height="20" fill="#111827" />

                <!-- Ring Canvas Floor -->
                <polygon points="40,260 760,260 800,600 0,600" fill="#3b82f6" />
                <polygon points="70,280 730,280 770,580 30,580" fill="#2563eb" />
                <circle cx="400" cy="430" r="90" fill="none" stroke="#1d4ed8" stroke-width="8" />
                <text x="400" y="435" text-anchor="middle" fill="#1e40af" font-size="20" font-family="'Press Start 2P'">WVBA</text>

                <!-- Ring Ropes -->
                <line x1="40" y1="260" x2="760" y2="260" stroke="#ef4444" stroke-width="6" />
                <line x1="45" y1="275" x2="755" y2="275" stroke="#ffffff" stroke-width="5" />
                <line x1="50" y1="290" x2="750" y2="290" stroke="#3b82f6" stroke-width="6" />

                <!-- Corner Posts -->
                <rect x="30" y="220" width="20" height="80" fill="#dc2626" />
                <rect x="750" y="220" width="20" height="80" fill="#dc2626" />

                <!-- REFEREE -->
                <g transform="translate(180, 240)">
                    <rect x="10" y="0" width="25" height="8" fill="#dc2626" />
                    <rect x="5" y="8" width="35" height="6" fill="#dc2626" />
                    <rect x="15" y="14" width="20" height="12" fill="#78350f" />
                    <rect x="20" y="14" width="15" height="14" fill="#fca5a5" />
                    <rect x="30" y="18" width="4" height="4" fill="#000" />
                    <rect x="25" y="22" width="12" height="4" fill="#78350f" />
                    <rect x="10" y="26" width="25" height="24" fill="#ffffff" />
                    <rect x="15" y="26" width="4" height="24" fill="#000000" />
                    <rect x="25" y="26" width="4" height="24" fill="#000000" />
                    <rect x="12" y="50" width="21" height="20" fill="#2563eb" />
                </g>

                <!-- MIKE TYSON -->
                <g :transform="tysonTransform" class="transition-transform duration-75">
                    <ellipse cx="70" cy="240" rx="65" ry="16" fill="rgba(0,0,0,0.4)" />
                    <path d="M 25 100 L 115 100 L 105 180 L 35 180 Z" fill="#78350f" />
                    <rect x="30" y="175" width="80" height="35" fill="#111827" />
                    <rect x="30" y="175" width="80" height="6" fill="#ffffff" />

                    <!-- Arms -->
                    <rect x="-10" y="105" width="40" height="65" fill="#78350f" rx="8" />
                    <circle cx="5" cy="175" r="22" fill="#dc2626" />
                    <rect x="110" y="105" width="40" height="65" fill="#78350f" rx="8" />
                    <circle cx="135" cy="175" r="22" fill="#dc2626" />

                    <!-- Head -->
                    <rect x="40" y="30" width="60" height="75" fill="#78350f" rx="12" />
                    <rect x="40" y="25" width="60" height="15" fill="#000000" />
                    <circle cx="55" cy="55" r="4" fill="#ffffff" />
                    <circle cx="56" cy="55" r="2" fill="#000000" />
                    <circle cx="85" cy="55" r="4" fill="#ffffff" />
                    <circle cx="84" cy="55" r="2" fill="#000000" />
                    <polygon points="70,35 60,55 80,55" fill="#ef4444" :opacity="tysonBlinkOpacity" />
                    <path d="M 42 42 L 52 42 L 48 60 Z" fill="#1e1b4b" opacity="0.8" />
                    <rect x="55" y="80" width="30" :height="tysonMouthHeight" fill="#000000" />
                    <rect x="40" y="205" width="25" height="35" fill="#78350f" />
                    <rect x="75" y="205" width="25" height="35" fill="#78350f" />
                </g>

                <!-- LITTLE MAC -->
                <g :transform="playerTransform" class="transition-transform duration-75">
                    <ellipse cx="60" cy="190" rx="45" ry="12" fill="rgba(0,0,0,0.5)" />
                    <rect x="30" y="140" width="22" height="45" fill="#fca5a5" />
                    <rect x="68" y="140" width="22" height="45" fill="#fca5a5" />
                    <rect x="25" y="175" width="28" height="18" fill="#111827" rx="3" />
                    <rect x="67" y="175" width="28" height="18" fill="#111827" rx="3" />
                    <rect x="25" y="105" width="70" height="42" fill="#16a34a" rx="4" />
                    <rect x="25" y="105" width="70" height="6" fill="#ffffff" />
                    <path d="M 30 45 L 90 45 L 85 110 L 35 110 Z" fill="#15803d" />
                    <rect x="42" y="10" width="36" height="38" fill="#fca5a5" rx="6" />
                    <path d="M 38 8 L 82 8 L 82 32 L 38 32 Z" fill="#000000" />

                    <!-- Left Arm & Glove -->
                    <rect x="5" y="45" width="26" height="45" fill="#fca5a5" rx="6" />
                    <circle :cy="leftGloveCy" cx="15" r="18" fill="#15803d" />

                    <!-- Right Arm & Glove -->
                    <rect x="89" y="45" width="26" height="45" fill="#fca5a5" rx="6" />
                    <circle :cy="rightGloveCy" cx="105" r="18" fill="#15803d" />
                </g>

                <!-- Front Ring Ropes -->
                <line x1="0" y1="580" x2="800" y2="580" stroke="#ef4444" stroke-width="8" />
                <line x1="0" y1="595" x2="800" y2="595" stroke="#ffffff" stroke-width="5" />
            </svg>
        </div>

        <!-- On-Screen Arcade Controls -->
        <div class="grid grid-cols-12 gap-2 bg-stone-950 border-2 border-stone-800 p-3 rounded-lg shrink-0">
            <div class="col-span-6 sm:col-span-5 grid grid-cols-3 gap-1 text-center max-w-[220px]">
                <div></div>
                <button @click="playerBlock" class="bg-stone-800 active:bg-yellow-600 text-xs p-2 rounded border border-stone-600 arcade-btn cursor-pointer">▲<br><span class="text-[8px]">BLOCK</span></button>
                <div></div>
                <button @click="playerDodge('left')" class="bg-stone-800 active:bg-yellow-600 text-xs p-2 rounded border border-stone-600 arcade-btn cursor-pointer">◀<br><span class="text-[8px]">LEFT</span></button>
                <button @click="playerDuck" class="bg-stone-800 active:bg-yellow-600 text-xs p-2 rounded border border-stone-600 arcade-btn cursor-pointer">▼<br><span class="text-[8px]">DUCK</span></button>
                <button @click="playerDodge('right')" class="bg-stone-800 active:bg-yellow-600 text-xs p-2 rounded border border-stone-600 arcade-btn cursor-pointer">▶<br><span class="text-[8px]">RIGHT</span></button>
            </div>

            <div class="hidden sm:flex col-span-2 flex-col justify-center text-[9px] text-gray-400 border-x border-stone-800 px-2 leading-relaxed">
                <div class="text-yellow-400 font-bold mb-1">KEYBOARD:</div>
                <div>←/→ : DODGE</div>
                <div>↑ : BLOCK | ↓ : DUCK</div>
                <div>Z / X : JABS</div>
                <div>SPACE : STAR PUNCH</div>
            </div>

            <div class="col-span-6 sm:col-span-5 flex flex-wrap gap-2 items-center justify-end">
                <button @click="playerPunch('left')" class="flex-1 bg-red-700 active:bg-red-800 text-white text-xs py-3 px-1 rounded border-2 border-red-400 arcade-btn font-bold cursor-pointer">
                    LEFT JAB<br><span class="text-[8px] text-red-200">(Z)</span>
                </button>
                <button @click="playerPunch('right')" class="flex-1 bg-red-700 active:bg-red-800 text-white text-xs py-3 px-1 rounded border-2 border-red-400 arcade-btn font-bold cursor-pointer">
                    RIGHT JAB<br><span class="text-[8px] text-red-200">(X)</span>
                </button>
                <button @click="playerStarPunch" class="w-full bg-yellow-500 active:bg-yellow-600 text-black text-xs py-2 px-2 rounded border-2 border-white arcade-btn font-extrabold cursor-pointer">
                    ★ STAR PUNCH ★ (SPACE)
                </button>
            </div>
        </div>

        <!-- Footer Credits -->
        <div class="mt-2 text-center text-[9px] text-gray-500 shrink-0">
            PUNCH-OUT!! ENDLESS FIGHT SIMULATION • FULLSCREEN VUE 3 PORT
        </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted, onUnmounted } from 'vue';

/* Retro Audio Engine */
class RetroAudioEngine {
    constructor() {
        this.ctx = null;
        this.enabled = true;
    }
    init() {
        if (!this.ctx) {
            const AudioContext = window.AudioContext || window.webkitAudioContext;
            this.ctx = new AudioContext();
        }
        if (this.ctx.state === 'suspended') this.ctx.resume();
    }
    playTone(freq, duration, type = 'square', volume = 0.15) {
        if (!this.enabled || !this.ctx) return;
        try {
            const osc = this.ctx.createOscillator();
            const gain = this.ctx.createGain();
            osc.type = type;
            osc.frequency.setValueAtTime(freq, this.ctx.currentTime);
            gain.gain.setValueAtTime(volume, this.ctx.currentTime);
            gain.gain.exponentialRampToValueAtTime(0.001, this.ctx.currentTime + duration);
            osc.connect(gain);
            gain.connect(this.ctx.destination);
            osc.start();
            osc.stop(this.ctx.currentTime + duration);
        } catch(e) {}
    }
    punchHit() {
        this.playTone(180, 0.08, 'square', 0.25);
        this.playTone(90, 0.12, 'sawtooth', 0.3);
    }
    dynamiteUppercut() {
        this.playTone(320, 0.05, 'sawtooth', 0.3);
        setTimeout(() => this.playTone(160, 0.1, 'square', 0.35), 40);
        setTimeout(() => this.playTone(60, 0.3, 'sawtooth', 0.4), 90);
    }
    dodgeSwoosh() {
        this.playTone(400, 0.05, 'triangle', 0.1);
        setTimeout(() => this.playTone(600, 0.05, 'triangle', 0.1), 30);
    }
    warningBlink() {
        this.playTone(880, 0.06, 'sine', 0.2);
    }
    starPunchSound() {
        [440, 554, 659, 880].forEach((freq, idx) => {
            setTimeout(() => this.playTone(freq, 0.08, 'square', 0.25), idx * 50);
        });
    }
    ringBell() {
        this.playTone(1200, 0.4, 'sine', 0.3);
        setTimeout(() => this.playTone(1200, 0.4, 'sine', 0.3), 150);
    }
}

const audio = new RetroAudioEngine();

/* Reactive Game State */
const crtEnabled = ref(true);
const soundEnabled = ref(true);
const isFullscreen = ref(false);
const timeRemaining = ref(90);
const round = ref(1);
const phase = ref(1);
const playerHp = ref(100);
const tysonHp = ref(100);
const stars = ref(0);
const playerHits = ref(0);
const tysonHits = ref(0);
const tysonStatus = ref("ENDLESS FURY");
const phaseIndicator = ref("PHASE 1: DYNAMITE");
const isShaking = ref(false);
const floatingTexts = ref([]);

/* Transforms & Animation variables */
const playerTransform = ref('translate(340, 360)');
const tysonTransform = ref('translate(330, 190)');
const tysonBlinkOpacity = ref(0);
const tysonMouthHeight = ref(6);
const leftGloveCy = ref(40);
const rightGloveCy = ref(40);

let timerInterval = null;
let tysonTimeout = null;
let isVulnerable = false;

const formattedTimer = computed(() => {
    const mins = Math.floor(timeRemaining.value / 60);
    const secs = timeRemaining.value % 60;
    return `${mins.toString().padStart(2, '0')}:${secs.toString().padStart(2, '0')}`;
});

/* Fullscreen handling */
const toggleFullscreen = () => {
    if (!document.fullscreenElement) {
        document.documentElement.requestFullscreen().catch(() => {});
        isFullscreen.value = true;
    } else {
        if (document.exitFullscreen) {
            document.exitFullscreen();
        }
        isFullscreen.value = false;
    }
};

const toggleScanlines = () => { crtEnabled.value = !crtEnabled.value; };
const toggleSound = () => {
    soundEnabled.value = !soundEnabled.value;
    audio.enabled = soundEnabled.value;
};

const showFloatingText = (text, color) => {
    const id = Date.now() + Math.random();
    floatingTexts.value.push({ id, text, color });
    setTimeout(() => {
        floatingTexts.value = floatingTexts.value.filter(f => f.id !== id);
    }, 800);
};

/* Game Loop & AI */
const initGame = () => {
    const enableAudio = () => {
        audio.init();
        window.removeEventListener('keydown', enableAudio);
        window.removeEventListener('click', enableAudio);
    };
    window.addEventListener('keydown', enableAudio);
    window.addEventListener('click', enableAudio);

    timerInterval = setInterval(() => {
        timeRemaining.value--;
        if (timeRemaining.value < 0) {
            timeRemaining.value = 90;
            round.value++;
            audio.ringBell();
            phase.value = phase.value === 1 ? 2 : 1;
            phaseIndicator.value = phase.value === 1 ? "PHASE 1: DYNAMITE" : "PHASE 2: WINK HOOKS & JABS";
            showFloatingText(`PHASE ${phase.value}!`, phase.value === 1 ? "text-red-400" : "text-yellow-400");
        }
    }, 1000);

    scheduleTyson();
};

const scheduleTyson = () => {
    const delay = Math.random() * 1400 + 1000;
    tysonTimeout = setTimeout(() => {
        if (phase.value === 1) {
            executeDynamite();
        } else {
            Math.random() > 0.4 ? executeHook() : executeJab();
        }
    }, delay);
};

const executeDynamite = () => {
    tysonStatus.value = "CHARGING UPPERCUT!";
    tysonBlinkOpacity.value = 1;
    audio.warningBlink();
    tysonTransform.value = 'translate(330, 175)';

    setTimeout(() => {
        tysonBlinkOpacity.value = 0;
        audio.dynamiteUppercut();
        tysonTransform.value = 'translate(330, 220)';
        isShaking.value = true;
        setTimeout(() => isShaking.value = false, 250);

        playerHp.value = Math.max(15, playerHp.value - 25);
        tysonHits.value++;
        showFloatingText("DYNAMITE HIT!", "text-red-500");

        setTimeout(() => {
            tysonTransform.value = 'translate(330, 190)';
            tysonStatus.value = "ENDLESS FURY";
            scheduleTyson();
        }, 400);
    }, 450);
};

const executeHook = () => {
    tysonStatus.value = "WINK HOOK!";
    audio.warningBlink();
    setTimeout(() => {
        audio.punchHit();
        tysonTransform.value = 'translate(360, 190)';
        playerHp.value = Math.max(15, playerHp.value - 15);
        showFloatingText("HOOK HIT!", "text-red-500");
        setTimeout(() => {
            tysonTransform.value = 'translate(330, 190)';
            scheduleTyson();
        }, 350);
    }, 380);
};

const executeJab = () => {
    tysonStatus.value = "FAST JAB!";
    audio.punchHit();
    tysonTransform.value = 'translate(330, 205)';
    playerHp.value = Math.max(15, playerHp.value - 10);
    setTimeout(() => {
        tysonTransform.value = 'translate(330, 190)';
        scheduleTyson();
    }, 250);
};

const playerDodge = (dir) => {
    audio.dodgeSwoosh();
    playerTransform.value = dir === 'left' ? 'translate(260, 370)' : 'translate(420, 370)';
    setTimeout(() => playerTransform.value = 'translate(340, 360)', 320);
};

const playerBlock = () => {
    audio.dodgeSwoosh();
    playerTransform.value = 'translate(340, 345)';
    setTimeout(() => playerTransform.value = 'translate(340, 360)', 300);
};

const playerDuck = () => {
    audio.dodgeSwoosh();
    playerTransform.value = 'translate(340, 390)';
    setTimeout(() => playerTransform.value = 'translate(340, 360)', 300);
};

const playerPunch = (side) => {
    if (side === 'left') leftGloveCy.value = 15;
    else rightGloveCy.value = 15;

    audio.punchHit();
    tysonHp.value = Math.max(15, tysonHp.value - 15);
    playerHits.value++;
    showFloatingText("HIT!", "text-yellow-400");

    if (Math.random() > 0.4 && stars.value < 3) {
        stars.value++;
        audio.starPunchSound();
        showFloatingText("+1 STAR!", "text-yellow-300");
    }

    setTimeout(() => {
        leftGloveCy.value = 40;
        rightGloveCy.value = 40;
    }, 220);
};

const playerStarPunch = () => {
    if (stars.value <= 0) return;
    stars.value--;
    audio.starPunchSound();
    playerTransform.value = 'translate(340, 330)';
    showFloatingText("★ STAR PUNCH ★", "text-yellow-400 text-lg");

    tysonHp.value = Math.max(15, tysonHp.value - 35);
    isShaking.value = true;
    setTimeout(() => isShaking.value = false, 250);

    setTimeout(() => {
        playerTransform.value = 'translate(340, 360)';
    }, 300);
};

const handleKeydown = (e) => {
    if (e.key === 'ArrowLeft') playerDodge('left');
    if (e.key === 'ArrowRight') playerDodge('right');
    if (e.key === 'ArrowUp') playerBlock();
    if (e.key === 'ArrowDown') playerDuck();
    if (e.key === 'z' || e.key === 'Z') playerPunch('left');
    if (e.key === 'x' || e.key === 'X') playerPunch('right');
    if (e.key === ' ') playerStarPunch();
};

onMounted(() => {
    window.addEventListener('keydown', handleKeydown);
    initGame();
});

onUnmounted(() => {
    window.removeEventListener('keydown', handleKeydown);
    clearInterval(timerInterval);
    clearTimeout(tysonTimeout);
});
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Press+Start+2P&display=swap');

.font-arcade {
    font-family: 'Press Start 2P', monospace;
}

.scanlines {
    background: linear-gradient(
        rgba(18, 16, 16, 0) 50%, 
        rgba(0, 0, 0, 0.35) 50%
    );
    background-size: 100% 4px;
    pointer-events: none;
}

@keyframes shake {
    0%, 100% { transform: translate(0, 0); }
    20% { transform: translate(-6px, 4px); }
    40% { transform: translate(6px, -4px); }
    60% { transform: translate(-4px, -2px); }
    80% { transform: translate(4px, 2px); }
}

.screen-shake {
    animation: shake 0.25s ease-in-out;
}

.floating-text {
    animation: floatUp 0.8s ease-out forwards;
}

@keyframes floatUp {
    0% { opacity: 1; transform: translateY(0) scale(1); }
    50% { transform: translateY(-20px) scale(1.3); }
    100% { opacity: 0; transform: translateY(-40px) scale(0.9); }
}

svg, image, canvas {
    image-rendering: pixelated;
    image-rendering: crisp-edges;
}

.arcade-btn {
    box-shadow: inset 0 -4px 0 rgba(0,0,0,0.4), 0 4px 0 #000;
}

.arcade-btn:active {
    transform: translateY(3px);
    box-shadow: inset 0 -1px 0 rgba(0,0,0,0.4), 0 1px 0 #000;
}
</style>