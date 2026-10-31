
<template>
  <div class="phone-wrapper">
    <!-- Smartphone Body Frame -->
    <div class="phone-frame" :data-theme="isDarkMode ? 'dark' : 'light'">
      
      <!-- iOS / Android Dynamic Notch -->
      <div class="dynamic-island">
        <div class="camera-lens"></div>
        <div class="sensor-dot"></div>
      </div>

      <!-- Smartphone Screen Display -->
      <div class="phone-screen">
        
        <!-- Status Bar -->
        <div class="status-bar">
          <span>{{ currentTime }}</span>
          <div class="status-icons">
            <i class="fas fa-signal"></i>
            <i class="fas fa-wifi"></i>
            <i class="fas fa-battery-full" style="color: #25d366;"></i>
          </div>
        </div>

        <!-- Interactive Banner Push Notification -->
        <div class="push-notification" :class="{ active: notifActive }" @click="dismissNotif">
          <div class="notif-header">
            <div class="notif-app">
              <i class="fab fa-whatsapp"></i>
              <span>WhatsApp</span>
            </div>
            <span class="notif-time">Just now</span>
          </div>
          <div class="notif-body">
            <div class="notif-avatar">🍜</div>
            <div class="notif-content">
              <h4>Holy Ramen</h4>
              <p>{{ currentNotifText }}</p>
            </div>
          </div>
        </div>

        <!-- WhatsApp Chat Header -->
        <div class="wa-header">
          <div class="wa-header-left">
            <i class="fas fa-chevron-left back-btn"></i>
            <div class="avatar-container">
              🍜
              <div class="avatar-badge"></div>
            </div>
            <div class="header-title-box">
              <span class="contact-name">
                Holy Ramen
                <i class="fas fa-check-circle verified-tick"></i>
              </span>
              <span class="contact-status" :class="{ typing: isTyping }">
                {{ isTyping ? 'typing...' : 'online' }}
              </span>
            </div>
          </div>
          <div class="wa-header-actions">
            <i class="fas fa-video"></i>
            <i class="fas fa-phone"></i>
            <i class="fas fa-ellipsis-vertical"></i>
          </div>
        </div>

        <!-- Chat Scroll Area -->
        <div class="chat-area" ref="chatAreaRef">
          <div class="date-pill-container">
            <span class="date-pill">Today</span>
          </div>

          <div class="encryption-notice">
            <i class="fas fa-lock" style="font-size: 9px; margin-right: 3px;"></i>
            Messages are end-to-end encrypted with rich broth & perfection.
          </div>

          <!-- Default Prompt Message -->
          <div class="message-row received">
            <div class="bubble received">
              <p>Bless us with the sacred noodle prayer! 🙏</p>
              <div class="bubble-meta">
                <span>12:17 PM</span>
              </div>
            </div>
          </div>

          <!-- Dynamic Reactive Message Thread -->
          <div v-for="(msg, idx) in messages" :key="idx" class="message-row" :class="msg.sender">
            
            <!-- Special Amen Final Line Styling -->
            <div v-if="msg.isAmen" class="bubble amen-special">
              <p>{{ msg.text }}</p>
            </div>

            <!-- Standard Prayer Bubbles -->
            <div v-else class="bubble" :class="msg.sender">
              <p :class="{ 'bubble-first-line': msg.isFirst }">{{ msg.text }}</p>
              <div class="bubble-meta">
                <span>{{ msg.time }}</span>
                <i v-if="msg.sender === 'sent'" class="fas fa-check-double ticks"></i>
              </div>
            </div>
          </div>

          <!-- Animated Typing Bubble Indicator -->
          <div v-if="isTyping" class="message-row received">
            <div class="typing-bubble">
              <div class="dot"></div>
              <div class="dot"></div>
              <div class="dot"></div>
            </div>
          </div>
        </div>

        <!-- WhatsApp Interactive Input Footer -->
        <div class="wa-footer">
          <i class="far fa-face-smile input-icon"></i>
          <i class="fas fa-paperclip input-icon"></i>
          <form class="input-form" @submit.prevent="sendUserMessage">
            <input 
              v-model="userInputValue" 
              type="text" 
              class="chat-input" 
              placeholder="Type 'Ramen'..." 
              autocomplete="off"
            />
          </form>
          <button class="send-btn" @click="sendUserMessage">
            <i class="fas fa-paper-plane" style="font-size: 12px;"></i>
          </button>
        </div>

        <!-- Bottom Gesture Home Bar -->
        <div class="home-bar">
          <div class="home-indicator"></div>
        </div>
      </div>
    </div>
  </div>
</template>



<script setup>
import { ref, computed, onMounted, onUnmounted, nextTick } from 'vue';

// Prayer lines reactive array
const prayerLines = [
  { text: "Our Father of noodles, rich broth, and endless simmer,", isFirst: true },
  { text: "Give us this day our daily bowl," },
  { text: "Topped high with tender pork, soft-boiled eggs, and green onions. 🍥🥚" },
  { text: "Lead us not into cold takeout or soggy noodles," },
  { text: "But deliver us to steaming perfection," },
  { text: "Slurping with glad and hungry hearts. 🍜✨" },
  { text: "For yours is the flavor, the warmth, and the comfort, now and forever." },
  { text: "Ramen. 🔥", isAmen: true }
];

// Reactive State Variables
const messages = ref([]);
const currentIndex = ref(0);
const isTyping = ref(false);
const isPlaying = ref(true);
const speed = ref(1); // 1x, 1.5x, 2x
const isDarkMode = ref(true);
const notifActive = ref(false);
const currentNotifText = ref('');
const userInputValue = ref('');
const currentTime = ref('');
const chatAreaRef = ref(null);

let stepTimeout = null;
let notifTimeout = null;
let clockInterval = null;

// Speed multiplier display text
const speedLabel = computed(() => `${speed.value}x Speed`);

// Clock update logic
const updateTime = () => {
  const now = new Date();
  let hours = now.getHours();
  let minutes = now.getMinutes();
  const ampm = hours >= 12 ? 'PM' : 'AM';
  hours = hours % 12 || 12;
  minutes = minutes < 10 ? '0' + minutes : minutes;
  currentTime.value = `${hours}:${minutes}`;
  return `${hours}:${minutes} ${ampm}`;
};

const scrollToBottom = () => {
  nextTick(() => {
    if (chatAreaRef.value) {
      chatAreaRef.value.scrollTop = chatAreaRef.value.scrollHeight;
    }
  });
};

// Sequential line animation engine
const runSequence = () => {
  if (!isPlaying.value) return;

  if (currentIndex.value >= prayerLines.length) {
    isTyping.value = false;
    isPlaying.value = false;
    return;
  }

  isTyping.value = true;
  scrollToBottom();

  const typingDuration = (1400 + Math.random() * 500) / speed.value;

  stepTimeout = setTimeout(() => {
    if (!isPlaying.value) return;

    isTyping.value = false;
    const lineItem = prayerLines[currentIndex.value];

    messages.value.push({
      sender: 'received',
      text: lineItem.text,
      isFirst: lineItem.isFirst || false,
      isAmen: lineItem.isAmen || false,
      time: updateTime()
    });

    triggerNotification(lineItem.text);
    scrollToBottom();

    currentIndex.value++;

    const pauseDuration = (1200 + Math.random() * 400) / speed.value;
    stepTimeout = setTimeout(runSequence, pauseDuration);
  }, typingDuration);
};

const triggerNotification = (text) => {
  clearTimeout(notifTimeout);
  currentNotifText.value = text;
  notifActive.value = true;
  notifTimeout = setTimeout(() => {
    notifActive.value = false;
  }, 3000 / speed.value);
};

const dismissNotif = () => {
  notifActive.value = false;
};

// Controls Action Methods
const togglePlayPause = () => {
  isPlaying.value = !isPlaying.value;
  if (isPlaying.value) {
    if (currentIndex.value >= prayerLines.length) {
      resetChat();
    } else {
      runSequence();
    }
  } else {
    clearTimeout(stepTimeout);
    isTyping.value = false;
  }
};

const cycleSpeed = () => {
  if (speed.value === 1) speed.value = 1.5;
  else if (speed.value === 1.5) speed.value = 2;
  else speed.value = 1;
};

const toggleTheme = () => {
  isDarkMode.value = !isDarkMode.value;
};

const resetChat = () => {
  clearTimeout(stepTimeout);
  clearTimeout(notifTimeout);
  notifActive.value = false;
  isTyping.value = false;
  messages.value = [];
  currentIndex.value = 0;
  isPlaying.value = true;
  runSequence();
};

const sendUserMessage = () => {
  const val = userInputValue.value.trim();
  if (!val) return;

  messages.value.push({
    sender: 'sent',
    text: val,
    time: updateTime()
  });
  userInputValue.value = '';
  scrollToBottom();
};

// Lifecycle hooks
onMounted(() => {
  updateTime();
  clockInterval = setInterval(updateTime, 30000);
  setTimeout(runSequence, 600);
});

onUnmounted(() => {
  clearTimeout(stepTimeout);
  clearTimeout(notifTimeout);
  clearInterval(clockInterval);
});
</script>

<style scoped>
/* Main Phone Component CSS Styles */
.phone-wrapper {
  display: flex;
  flex-direction: column;
  align-items: left;
  font-family: 'Segoe UI', -apple-system, BlinkMacSystemFont, Roboto, sans-serif;
}

.controls-bar {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
  flex-wrap: wrap;
  justify-content: center;
}

.ctrl-btn {
  background-color: #1e293b;
  color: #f8fafc;
  border: none;
  padding: 8px 16px;
  border-radius: 20px;
  font-size: 13px;
  font-weight: 600;
  cursor: pointer;
  display: flex;
  align-items: center;
  gap: 8px;
  transition: all 0.2s ease;
  box-shadow: 0 2px 5px rgba(0,0,0,0.2);
}

.ctrl-btn:hover {
  background-color: #334155;
  transform: translateY(-1px);
}

.ctrl-btn-primary {
  background-color: #25d366;
  color: #ffffff;
}

.ctrl-btn-primary:hover {
  background-color: #22c55e;
}

/* Smartphone Mockup */
.phone-frame {
  position: relative;
  width: 380px;
  height: 710px;
  background-color: #000;
  border-radius: 46px;
  padding: 10px;
  border: 5px solid #1e293b;
  box-shadow: 0 25px 50px -12px rgba(0, 0, 0, 0.6), 0 0 30px rgba(37, 211, 102, 0.15);
  display: flex;
  flex-direction: column;
  overflow: hidden;
  transition: all 0.3s ease;
  margin-left: 10px;
  margin-top: 10px;
}

.phone-screen {
  position: relative;
  width: 100%;
  height: 100%;
  border-radius: 36px;
  overflow: hidden;
  display: flex;
  flex-direction: column;
  background-color: var(--wa-chat-bg);
}

/* Dynamic Island Notch */
.dynamic-island {
  position: absolute;
  top: 10px;
  left: 50%;
  transform: translateX(-50%);
  width: 110px;
  height: 24px;
  background-color: #000;
  border-bottom-left-radius: 16px;
  border-bottom-right-radius: 16px;
  z-index: 60;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
}

.camera-lens {
  width: 9px;
  height: 9px;
  border-radius: 50%;
  background-color: #111;
  border: 1px solid #222;
}

.sensor-dot {
  width: 6px;
  height: 6px;
  border-radius: 50%;
  background-color: #0c1017;
}

/* Status Bar */
.status-bar {
  width: 100%;
  padding: 8px 22px 4px 22px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  font-size: 11px;
  font-weight: 600;
  color: var(--wa-header-text);
  background-color: var(--wa-header-bg);
  z-index: 40;
}

.status-icons {
  display: flex;
  align-items: center;
  gap: 6px;
}

/* Theme color definitions via scoped data attributes */
.phone-frame[data-theme="dark"] {
  --wa-header-bg: #202c33;
  --wa-header-text: #e9edef;
  --wa-subtext: #8696a0;
  --wa-chat-bg: #0b141a;
  --wa-pattern: #202c33;
  --wa-rec-bg: #202c33;
  --wa-rec-text: #e9edef;
  --wa-sent-bg: #005c4b;
  --wa-sent-text: #e9edef;
  --wa-input-bg: #202c33;
  --wa-input-field: #2a3942;
  --wa-input-text: #d1d5db;
  --notif-bg: rgba(30, 41, 59, 0.95);
  --notif-text: #f8fafc;
}

.phone-frame[data-theme="light"] {
  --wa-header-bg: #075e54;
  --wa-header-text: #ffffff;
  --wa-subtext: #d1fae5;
  --wa-chat-bg: #efeae2;
  --wa-pattern: #cbd5e1;
  --wa-rec-bg: #ffffff;
  --wa-rec-text: #111b21;
  --wa-sent-bg: #d9fdd3;
  --wa-sent-text: #111b21;
  --wa-input-bg: #f0f2f5;
  --wa-input-field: #ffffff;
  --wa-input-text: #111b21;
  --notif-bg: rgba(255, 255, 255, 0.95);
  --notif-text: #0f172a;
}

/* Push Notification */
.push-notification {
  position: absolute;
  top: 36px;
  left: 10px;
  right: 10px;
  background-color: var(--notif-bg);
  color: var(--notif-text);
  border-radius: 16px;
  padding: 10px 14px;
  z-index: 70;
  box-shadow: 0 10px 25px rgba(0,0,0,0.3);
  backdrop-filter: blur(10px);
  cursor: pointer;
  transform: translateY(-150%);
  opacity: 0;
  transition: transform 0.4s cubic-bezier(0.16, 1, 0.3, 1), opacity 0.4s ease;
}

.push-notification.active {
  transform: translateY(0);
  opacity: 1;
}

.notif-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 4px;
}

.notif-app {
  display: flex;
  align-items: center;
  gap: 6px;
  font-size: 10px;
  font-weight: 700;
  color: #25d366;
  text-transform: uppercase;
}

.notif-time {
  font-size: 10px;
  opacity: 0.6;
}

.notif-body {
  display: flex;
  align-items: flex-start;
  gap: 10px;
}

.notif-avatar {
  width: 32px;
  height: 32px;
  border-radius: 50%;
  background: linear-gradient(135deg, #f59e0b, #d97706);
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 16px;
  flex-shrink: 0;
}

.notif-content h4 {
  font-size: 12px;
  font-weight: 700;
}

.notif-content p {
  font-size: 11px;
  opacity: 0.85;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

/* WhatsApp Header */
.wa-header {
  background-color: var(--wa-header-bg);
  color: var(--wa-header-text);
  padding: 8px 12px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  z-index: 30;
}

.wa-header-left {
  display: flex;
  align-items: center;
  gap: 8px;
}

.back-btn {
  font-size: 13px;
  cursor: pointer;
}

.avatar-container {
  position: relative;
  width: 36px;
  height: 36px;
  border-radius: 50%;
  background: linear-gradient(135deg, #f59e0b, #d97706);
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 16px;
}

.avatar-badge {
  position: absolute;
  bottom: 0;
  right: 0;
  width: 9px;
  height: 9px;
  background-color: #25d366;
  border-radius: 50%;
  border: 2px solid var(--wa-header-bg);
}

.header-title-box {
  display: flex;
  flex-direction: column;
}

.contact-name {
  font-size: 13px;
  font-weight: 600;
  display: flex;
  align-items: center;
  gap: 4px;
}

.verified-tick {
  color: #25d366;
  font-size: 10px;
}

.contact-status {
  font-size: 10px;
  color: var(--wa-subtext);
}

.contact-status.typing {
  color: #25d366;
  font-weight: 600;
}

.wa-header-actions {
  display: flex;
  gap: 14px;
  font-size: 13px;
}

/* Chat Body */
.chat-area {
  flex: 1;
  padding: 10px;
  overflow-y: auto;
  display: flex;
  flex-direction: column;
  gap: 8px;
  background-color: var(--wa-chat-bg);
  background-image: radial-gradient(var(--wa-pattern) 1px, transparent 0);
  background-size: 18px 18px;
}

.date-pill-container {
  display: flex;
  justify-content: center;
  margin: 4px 0;
}

.date-pill {
  background-color: rgba(32, 44, 51, 0.7);
  color: #8696a0;
  font-size: 9px;
  font-weight: 600;
  padding: 3px 8px;
  border-radius: 6px;
  text-transform: uppercase;
}

.encryption-notice {
  background-color: rgba(254, 243, 199, 0.1);
  border: 1px solid rgba(245, 158, 11, 0.2);
  color: #fbbf24;
  font-size: 9px;
  text-align: center;
  padding: 5px 10px;
  border-radius: 8px;
  margin: 0 auto 6px auto;
  max-width: 90%;
}

/* Message Bubbles */
.message-row {
  display: flex;
  width: 100%;
  animation: fadeInBubble 0.3s ease forwards;
}

@keyframes fadeInBubble {
  from { opacity: 0; transform: translateY(8px); }
  to { opacity: 1; transform: translateY(0); }
}

.message-row.received { justify-content: flex-start; }
.message-row.sent { justify-content: flex-end; }

.bubble {
  max-width: 82%;
  padding: 7px 11px;
  border-radius: 10px;
  font-size: 12.5px;
  line-height: 1.35;
  box-shadow: 0 1px 2px rgba(0,0,0,0.15);
}

.bubble.received {
  background-color: var(--wa-rec-bg);
  color: var(--wa-rec-text);
  border-top-left-radius: 2px;
}

.bubble.sent {
  background-color: var(--wa-sent-bg);
  color: var(--wa-sent-text);
  border-top-right-radius: 2px;
}

.bubble.amen-special {
  background-color: rgba(15, 44, 36, 0.9);
  border: 1.5px solid #00a884;
  text-align: center;
  font-family: 'Outfit', sans-serif;
  font-weight: 700;
  font-size: 15px;
  color: #25d366;
  padding: 8px 14px;
  max-width: 75%;
  margin: 4px auto;
  border-radius: 14px;
}

.bubble-first-line {
  font-family: 'Outfit', sans-serif;
  font-style: italic;
  border-left: 2px solid #f59e0b;
  padding-left: 6px;
  color: #fde68a;
}

.bubble-meta {
  display: flex;
  align-items: center;
  justify-content: flex-end;
  gap: 3px;
  font-size: 8.5px;
  color: var(--wa-subtext);
  margin-top: 3px;
}

.ticks {
  color: #38bdf8;
  font-size: 9px;
}

/* Typing Dots */
.typing-bubble {
  background-color: var(--wa-rec-bg);
  padding: 8px 12px;
  border-radius: 10px;
  border-top-left-radius: 2px;
  display: inline-flex;
  gap: 4px;
}

.dot {
  width: 5px;
  height: 5px;
  background-color: #25d366;
  border-radius: 50%;
  animation: bounce 1.3s infinite ease-in-out;
}

.dot:nth-child(2) { animation-delay: 0.2s; }
.dot:nth-child(3) { animation-delay: 0.4s; }

@keyframes bounce {
  0%, 60%, 100% { transform: translateY(0); }
  30% { transform: translateY(-4px); }
}

/* Input Bar */
.wa-footer {
  background-color: var(--wa-input-bg);
  padding: 6px 8px;
  display: flex;
  align-items: center;
  gap: 6px;
  z-index: 30;
}

.input-icon {
  color: var(--wa-subtext);
  font-size: 16px;
  cursor: pointer;
  padding: 2px;
}

.input-form { flex: 1; }

.chat-input {
  width: 100%;
  background-color: var(--wa-input-field);
  color: var(--wa-input-text);
  border: none;
  outline: none;
  padding: 7px 12px;
  border-radius: 18px;
  font-size: 12px;
}

.send-btn {
  width: 32px;
  height: 32px;
  border-radius: 50%;
  background-color: #25d366;
  color: #fff;
  border: none;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
}

.home-bar {
  width: 100%;
  height: 10px;
  background-color: var(--wa-input-bg);
  display: flex;
  justify-content: center;
  align-items: center;
}

.home-indicator {
  width: 90px;
  height: 3.5px;
  background-color: var(--wa-subtext);
  border-radius: 4px;
  opacity: 0.4;
}
</style>