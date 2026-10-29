<template>
  <header class="top-header-bar">
    <!-- Top Edge Full-Width Progress Bar -->
    <div
      class="progress-track"
      @click="seekTrack"
      @mousemove="handleProgressHover"
      @mouseleave="hoverProgress = null"
    >
      <div
        class="progress-fill"
        :style="{ width: progressPercent + '%' }"
      ></div>
      <div
        class="progress-handle"
        :style="{ left: progressPercent + '%' }"
      ></div>

      <!-- Hover Time Tooltip -->
      <div
        v-if="hoverProgress !== null"
        class="time-tooltip"
        :style="{ left: hoverProgress.x + 'px' }"
      >
        {{ formatTime(hoverProgress.time) }}
      </div>
    </div>

    <!-- Main Header Bar Contents -->
    <div class="header-content">
      <!-- Left: Track Info & Play/Pause -->
      <div class="track-section">
        <button
          @click="togglePlay"
          class="play-btn"
          :aria-label="isPlaying ? 'Pause' : 'Play'"
        >
          <span class="icon">{{ isPlaying ? "❚❚" : "▶" }}</span>
        </button>

        <!-- Clickable YouTube SVG Icon -->
        <a
          :href="currentTrack.href"
          target="_blank"
          rel="noopener noreferrer"
          class="youtube-logo-wrapper"
          :class="{ 'is-playing': isPlaying }"
          title="Watch on YouTube"
        >
          <svg
            class="youtube-svg"
            viewBox="0 0 461.001 461.001"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path
              fill="#F61C0D"
              d="M365.257,67.393H95.744C42.866,67.393,0,110.259,0,163.137v134.728
              c0,52.878,42.866,95.744,95.744,95.744h269.513c52.878,0,95.744-42.866,95.744-95.744V163.137
              C461.001,110.259,418.135,67.393,365.257,67.393z M300.506,237.056l-126.06,60.123c-3.359,1.602-7.239-0.847-7.239-4.568V168.607
              c0-3.774,3.982-6.22,7.348-4.514l126.06,63.881C304.363,229.873,304.298,235.248,300.506,237.056z"
            />
          </svg>
        </a>

        <div class="track-info">
          <a
            :href="currentTrack.href"
            target="_blank"
            rel="noopener noreferrer"
            class="track-title-link"
          >
            <span class="track-title">{{ currentTrack.name }}</span>
          </a>
          <span class="track-artist">{{ currentTrack.artist }}</span>
        </div>
      </div>

      <!-- Center/Right Controls: Volume & Playlist (Before time-section) -->
      <div class="controls-right-section">
        <!-- Triangle Volume Control -->
        <div class="volumeBar" @click="setVolume" title="Adjust Volume">
          <div class="volumebkg"></div>
          <div
            class="volume"
            :style="{ clip: `rect(0px, ${volume * 54}px, 30px, 0px)` }"
          ></div>
        </div>

        <!-- Playlist Container -->
        <div class="playlist-container">
          <button
            @click="showPlaylist = !showPlaylist"
            class="playlist-btn"
            :class="{ active: showPlaylist }"
            aria-label="Toggle Playlist"
            title="Playlist"
          >
            <svg
              class="playlist-icon"
              viewBox="0 0 24 24"
              fill="none"
              stroke="currentColor"
              stroke-width="2"
              stroke-linecap="round"
              stroke-linejoin="round"
            >
              <line x1="8" y1="6" x2="21" y2="6"></line>
              <line x1="8" y1="12" x2="21" y2="12"></line>
              <line x1="8" y1="18" x2="21" y2="18"></line>
              <line x1="3" y1="6" x2="3.01" y2="6"></line>
              <line x1="3" y1="12" x2="3.01" y2="12"></line>
              <line x1="3" y1="18" x2="3.01" y2="18"></line>
            </svg>
          </button>

          <!-- Playlist Dropdown Menu -->
          <div v-if="showPlaylist" class="playlist-menu">
            <div class="playlist-header">Playlist</div>
            <ul class="playlist-list">
              <li
                v-for="(track, idx) in tracks"
                :key="track.id"
                class="playlist-item"
                :class="{ 'is-active': idx === currentIndex }"
                @click="selectTrack(idx)"
              >
                <span class="playing-mark" v-if="idx === currentIndex">▶</span>
                <div class="playlist-item-info">
                  <span class="playlist-item-name">{{ track.name }}</span>
                  <span class="playlist-item-artist">{{ track.artist }}</span>
                </div>
              </li>
            </ul>
          </div>
        </div>
      </div>

      <!-- Right: Time & Status Indicator -->
      <div class="time-section">
        <div class="playing-indicator" :class="{ active: isPlaying }">
          <span></span><span></span><span></span>
        </div>
        <span class="time-display">
          {{ formatTime(currentTime) }} / {{ formatTime(duration) }}
        </span>
      </div>
    </div>
  </header>
</template>

<script setup>
import { ref, computed, onMounted, onUnmounted, watch } from "vue";
const emit = defineEmits(["startedSong"]);
const props = defineProps({
  index: {
    type: Number,
    required: false,
    default: 0,
  },
});
const tracks = ref([
  {
    id: 1,
    name: "Dock of the Bay",
    artist: "IMY2",
    href: "https://www.youtube.com/watch?v=Q-Bl7LzC_Bk",
    source:
      "https://github.com/thewongandonly/thewongandonly.github.io/raw/refs/heads/master/dockofthebay-otis-redding-cover.mp3",
  },
  {
    id: 2,
    name: "Sandstorm (Extended Mix)",
    artist: "Tryger",
    href: "https://www.youtube.com/watch?v=iTpk88R0IRg",
    source:
      "https://github.com/thewongandonly/thewongandonly.github.io/raw/refs/heads/master/melodic-techno.mp3",
  },
  {
    id: 3,
    name: "Deep Work Frequency: 40Hz Focus Mode for Productivity",
    artist: "VYBFRQ",
    href: "https://www.youtube.com/shorts/_pQIuTCQr50",
    source:
      "https://github.com/thewongandonly/thewongandonly.github.io/raw/refs/heads/master/DeepFocusMode.mp3",
  },
  {
    id: 4,
    name: "Doki Doki Literature Club! OST - Doki Doki Literature Club!",
    artist: "Team Salvato",
    href: "https://youtu.be/BFSWlDpA6C4",
    source:
      "https://github.com/thewongandonly/thewongandonly.github.io/raw/refs/heads/master/BGM03g.mp3",
  },
]);

const currentIndex = ref(0);
const isPlaying = ref(false);
const currentTime = ref(0);
const duration = ref(0);
const hoverProgress = ref(null);
const showPlaylist = ref(false);
const volume = ref(0.314); // 0 to 1

let audio = null;

const currentTrack = computed(() => tracks.value[currentIndex.value]);
const progressPercent = computed(() =>
  duration.value ? (currentTime.value / duration.value) * 100 : 0,
);

watch(
  () => props.index,
  (newVal, oldVal) => {
    selectTrack(newVal, false);
  },
);

onMounted(() => {
  audio = new Audio();
  audio.src = currentTrack.value.source;
  audio.volume = volume.value;

  audio.addEventListener("timeupdate", () => {
    currentTime.value = audio.currentTime || 0;
  });

  audio.addEventListener("loadedmetadata", () => {
    duration.value = audio.duration || 0;
  });

  audio.addEventListener("ended", () => {
    isPlaying.value = false;
    currentTime.value = 0;
  });
});

onUnmounted(() => {
  if (audio) {
    audio.pause();
    audio = null;
  }
});

const setVolume = (e) => {
  const rect = e.currentTarget.getBoundingClientRect();
  const clickX = e.clientX - rect.left;
  const newVol = Math.max(0, Math.min(1, clickX / rect.width));
  volume.value = newVol;
  if (audio) {
    audio.volume = volume.value;
  }
};

const selectTrack = (newIndex, autoplay = true) => {
  if (
    newIndex < 0 ||
    newIndex >= tracks.value.length ||
    currentIndex.value === newIndex
  )
    return;
  currentIndex.value = newIndex;
  currentTime.value = 0;
  showPlaylist.value = false;

  if (audio) {
    audio.src = currentTrack.value.source;
    audio.volume = volume.value;
    if (autoplay || isPlaying.value) {
      audio
        .play()
        .then(() => {
          isPlaying.value = true;
          emit("startedSong", currentIndex.value);
        })
        .catch(() => {
          isPlaying.value = false;
        });
    }
  }
};

const togglePlay = () => {
  if (!audio) return;
  if (isPlaying.value) {
    audio.pause();
    isPlaying.value = false;
  } else {
    audio.play().catch(() => {
      isPlaying.value = false;
    });
    isPlaying.value = true;
    emit("startedSong", currentIndex.value);
  }
};

const seekTrack = (e) => {
  if (!audio || !duration.value) return;
  const rect = e.currentTarget.getBoundingClientRect();
  const clickX = e.clientX - rect.left;
  const percentage = Math.max(0, Math.min(1, clickX / rect.width));
  audio.currentTime = percentage * duration.value;
};

const handleProgressHover = (e) => {
  if (!duration.value) return;
  const rect = e.currentTarget.getBoundingClientRect();
  const hoverX = e.clientX - rect.left;
  const percentage = Math.max(0, Math.min(1, hoverX / rect.width));
  hoverProgress.value = {
    x: hoverX,
    time: percentage * duration.value,
  };
};

const formatTime = (sec) => {
  if (!sec || isNaN(sec)) return "0:00";
  const minutes = Math.floor(sec / 60);
  const seconds = Math.floor(sec % 60);
  return `${minutes}:${seconds.toString().padStart(2, "0")}`;
};
</script>

<style scoped>
/* Top Header Bar Container with Slide-Down Entry Animation */
.top-header-bar {
  position: relative;
  width: 100%;
  background: #ffffff;
  border-bottom: 1px solid #e2e8f0;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.03);
  user-select: none;
  font-family:
    -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial,
    sans-serif;
  overflow: visible;
  z-index: 999999999;
  /* Slide down entry animation */
  animation: slideDown 5.6s cubic-bezier(0.16, 1, 0.3, 1) forwards;
}

@keyframes slideDown {
  0% {
    transform: translateY(-100%);
    opacity: 0;
  }
  100% {
    transform: translateY(0);
    opacity: 1;
  }
}

/* Interactive Progress Bar at Top Border */
.progress-track {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 4px;
  background: #edf2f7;
  cursor: pointer;
  transition: height 0.15s ease;
  z-index: 10;
}

.progress-track:hover {
  height: 6px;
}

.progress-fill {
  height: 100%;
  background: linear-gradient(90deg, #ff7675, #ee5253);
  transition: width 0.1s linear;
}

.progress-handle {
  position: absolute;
  top: 50%;
  transform: translate(-50%, -50%) scale(0);
  width: 10px;
  height: 10px;
  background-color: #ee5253;
  border-radius: 50%;
  transition: transform 0.15s ease;
  pointer-events: none;
}

.progress-track:hover .progress-handle {
  transform: translate(-50%, -50%) scale(1);
}

/* Time Tooltip on Hover */
.time-tooltip {
  position: absolute;
  top: 10px;
  transform: translateX(-50%);
  background: #2d3748;
  color: #fff;
  font-size: 11px;
  padding: 2px 6px;
  border-radius: 4px;
  pointer-events: none;
  white-space: nowrap;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.15);
}

/* Main Content Area */
.header-content {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 10px 16px;
  height: 56px;
  box-sizing: border-box;
}

/* Track & Play Button Section */
.track-section {
  display: flex;
  align-items: center;
  gap: 12px;
}

.play-btn {
  width: 36px;
  height: 36px;
  border-radius: 50%;
  border: none;
  background: #f1f5f9;
  color: #2d3748;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: all 0.2s ease;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.05);
}

.play-btn:hover {
  background: #ee5253;
  color: #ffffff;
  transform: scale(1.05);
}

.play-btn:active {
  transform: scale(0.95);
}

.play-btn .icon {
  font-size: 12px;
  margin-left: 1px;
}

/* Clickable YouTube SVG Container */
.youtube-logo-wrapper {
  width: 36px;
  height: 36px;
  border-radius: 8px;
  background: #f8fafc;
  border: 1px solid #f1f5f9;
  display: flex;
  align-items: center;
  justify-content: center;
  transition:
    transform 0.2s ease,
    box-shadow 0.2s ease,
    border-color 0.2s ease;
  text-decoration: none;
}

.youtube-logo-wrapper:hover {
  transform: scale(1.05);
  border-color: rgba(246, 28, 13, 0.4);
}

.youtube-logo-wrapper.is-playing {
  box-shadow: 0 4px 10px rgba(246, 28, 13, 0.25);
  border-color: rgba(246, 28, 13, 0.2);
}

.youtube-svg {
  width: 24px;
  height: 24px;
  display: block;
}

.track-info {
  display: flex;
  flex-direction: column;
}

.track-title-link {
  text-decoration: none;
}

.track-title-link:hover .track-title {
  color: #ee5253;
  text-decoration: underline;
}

.track-title {
  font-size: 13px;
  font-weight: 600;
  color: #1a202c;
  line-height: 1.2;
  transition: color 0.2s ease;
}

.track-artist {
  font-size: 11px;
  color: #718096;
  margin-top: 2px;
}

/* Controls Right Section (Volume & Playlist grouped together before time-section) */
.controls-right-section {
  display: flex;
  align-items: center;
  gap: 12px;
  margin-left: auto;
  margin-right: 12px;
}

/* Triangle Volume Control Styles */
.volumeBar {
  display: block;
  height: 30px;
  position: relative;
  background-color: transparent;
  z-index: 100;
  width: 54px;
  cursor: pointer;
}

.volume {
  position: absolute;
  top: 0;
  left: 0;
  width: 54px;
  height: 0;
  border-style: solid;
  border-width: 0 0 30px 54px;
  border-color: transparent transparent #f61c0d transparent;
  line-height: 0px;
}

.volumebkg {
  position: absolute;
  top: 0;
  left: 0;
  width: 0;
  height: 0;
  border-style: solid;
  border-width: 0 0 30px 54px;
  border-color: transparent transparent #cbd5e0 transparent;
  line-height: 0px;
}

/* Playlist Toggle & Dropdown Menu */
.playlist-container {
  position: relative;
  z-index: 999999;
}

.playlist-btn {
  width: 32px;
  height: 32px;
  border-radius: 6px;
  border: 1px solid #e2e8f0;
  background: #f8fafc;
  color: #4a5568;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: all 0.2s ease;
}

.playlist-btn:hover,
.playlist-btn.active {
  background: #edf2f7;
  color: #ee5253;
  border-color: #cbd5e0;
}

.playlist-icon {
  width: 18px;
  height: 18px;
}

.playlist-menu {
  position: absolute;
  top: 40px;
  right: 0;
  width: 240px;
  background: #ffffff;
  border: 1px solid #e2e8f0;
  border-radius: 8px;
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.1);
  z-index: 9999999;
  overflow: hidden;
}

.playlist-header {
  padding: 8px 12px;
  font-size: 11px;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 0.5px;
  color: #a0aec0;
  background: #f7fafc;
  border-bottom: 1px solid #edf2f7;
}

.playlist-list {
  list-style: none;
  margin: 0;
  padding: 4px 0;
}

.playlist-item {
  display: flex;
  align-items: center;
  padding: 8px 12px;
  cursor: pointer;
  transition: background 0.15s ease;
  gap: 8px;
}

.playlist-item:hover {
  background: #f7fafc;
}

.playlist-item.is-active {
  background: #fff5f5;
}

.playing-mark {
  color: #ee5253;
  font-size: 10px;
}

.playlist-item-info {
  display: flex;
  flex-direction: column;
}

.playlist-item-name {
  font-size: 12px;
  font-weight: 600;
  color: #2d3748;
}

.playlist-item.is-active .playlist-item-name {
  color: #ee5253;
}

.playlist-item-artist {
  font-size: 10px;
  color: #718096;
}

/* Time & Animated Sound Waves */
.time-section {
  display: flex;
  align-items: center;
  gap: 10px;
}

.time-display {
  font-size: 12px;
  font-weight: 500;
  color: #a0aec0;
  font-variant-numeric: tabular-nums;
}

/* Equalizer Visualizer Bars */
.playing-indicator {
  display: flex;
  align-items: flex-end;
  gap: 2px;
  height: 12px;
}

.playing-indicator span {
  width: 3px;
  height: 3px;
  background-color: #ee5253;
  border-radius: 1px;
  transition: height 0.2s ease;
}

.playing-indicator.active span:nth-child(1) {
  animation: bounce 0.6s ease infinite alternate;
}
.playing-indicator.active span:nth-child(2) {
  animation: bounce 0.6s ease infinite alternate 0.2s;
}
.playing-indicator.active span:nth-child(3) {
  animation: bounce 0.6s ease infinite alternate 0.4s;
}

@keyframes bounce {
  0% {
    height: 3px;
  }
  100% {
    height: 12px;
  }
}
</style>
