<template>
  <div class="sidebar">
    <div class="sidebar-backdrop" @click="closeSidebarPanel" v-if="isPanelOpen">
      <div class="close">
        <svg
          width="30px"
          height="30px"
          viewBox="0 0 24 24"
          fill="none"
          xmlns="http://www.w3.org/2000/svg"
        >
          <path
            d="M12 22C17.5 22 22 17.5 22 12C22 6.5 17.5 2 12 2C6.5 2 2 6.5 2 12C2 17.5 6.5 22 12 22Z"
            stroke="#292D32"
            stroke-width="1.5"
            stroke-linecap="round"
            stroke-linejoin="round"
          />
          <path
            d="M9.16998 14.83L14.83 9.17004"
            stroke="#292D32"
            stroke-width="1.5"
            stroke-linecap="round"
            stroke-linejoin="round"
          />
          <path
            d="M14.83 14.83L9.16998 9.17004"
            stroke="#292D32"
            stroke-width="1.5"
            stroke-linecap="round"
            stroke-linejoin="round"
          />
        </svg>
      </div>
    </div>
    <div v-if="!isPanelOpen" class="wishtochat" @click="isPanelOpen = true">
      <svg
        fill="#6e3bc2"
        version="1.1"
        id="Capa_1"
        xmlns="http://www.w3.org/2000/svg"
        xmlns:xlink="http://www.w3.org/1999/xlink"
        width="40px"
        height="40px"
        viewBox="0 0 30.743 30.744"
        xml:space="preserve"
      >
        <g>
          <path
            d="M28.585,9.67h-0.842v9.255c0,1.441-0.839,2.744-2.521,2.744H8.743v0.44c0,1.274,1.449,2.56,2.937,2.56h12.599l4.82,2.834
		L28.4,24.669h0.185c1.487,0,2.158-1.283,2.158-2.56V11.867C30.743,10.593,30.072,9.67,28.585,9.67z"
          />
          <path
            d="M22.762,3.24H3.622C1.938,3.24,0,4.736,0,6.178v11.6c0,1.328,1.642,2.287,3.217,2.435l-1.025,3.891L8.76,20.24h14.002
		c1.684,0,3.238-1.021,3.238-2.462V8.393V6.178C26,4.736,24.445,3.24,22.762,3.24z M6.542,13.032c-0.955,0-1.729-0.774-1.729-1.729
		s0.774-1.729,1.729-1.729c0.954,0,1.729,0.774,1.729,1.729S7.496,13.032,6.542,13.032z M13,13.032
		c-0.955,0-1.729-0.774-1.729-1.729S12.045,9.574,13,9.574s1.729,0.774,1.729,1.729S13.955,13.032,13,13.032z M19.459,13.032
		c-0.955,0-1.73-0.774-1.73-1.729s0.775-1.729,1.73-1.729c0.953,0,1.729,0.774,1.729,1.729S20.412,13.032,19.459,13.032z"
          />
        </g>
      </svg>
    </div>
    <transition name="slide">
      <div v-if="isPanelOpen" class="sidebar-panel">
        <div class="chat --dark-theme">
          <div class="chat__conversation-board">
            <div
              class="chat__conversation-board__message-container"
              v-for="(user, index) in chatconvo"
              :key="index"
              :class="{ reversed: !!user.you }"
            >
              <div
                v-if="!!user.text"
                class="chat__conversation-board__message__person"
              >
                <div class="chat__conversation-board__message__person__avatar">
                  <img
                    src="https://thewongandonly.com/wongbw.png"
                    height="44"
                    alt="David Wong"
                  />
                </div>
                <span
                  class="chat__conversation-board__message__person__nickname"
                  >David Wong</span
                >
              </div>
              <div
                v-if="!!user.text"
                class="chat__conversation-board__message__context"
              >
                <div class="chat__conversation-board__message__bubble">
                  <span>{{ user.text }}</span>
                </div>
              </div>
              <div
                v-if="!!user.text"
                class="chat__conversation-board__message__options"
              >
                <button
                  class="btn-icon chat__conversation-board__message__option-button option-item emoji-button"
                >
                  <svg
                    class="feather feather-smile sc-dnqmqq jxshSx"
                    xmlns="http://www.w3.org/2000/svg"
                    width="24"
                    height="24"
                    viewBox="0 0 24 24"
                    fill="none"
                    stroke="currentColor"
                    stroke-width="2"
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    aria-hidden="true"
                  >
                    <circle cx="12" cy="12" r="10"></circle>
                    <path d="M8 14s1.5 2 4 2 4-2 4-2"></path>
                    <line x1="9" y1="9" x2="9.01" y2="9"></line>
                    <line x1="15" y1="9" x2="15.01" y2="9"></line>
                  </svg>
                </button>
                <button
                  class="btn-icon chat__conversation-board__message__option-button option-item more-button"
                >
                  <svg
                    class="feather feather-more-horizontal sc-dnqmqq jxshSx"
                    xmlns="http://www.w3.org/2000/svg"
                    width="24"
                    height="24"
                    viewBox="0 0 24 24"
                    fill="none"
                    stroke="currentColor"
                    stroke-width="2"
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    aria-hidden="true"
                  >
                    <circle cx="12" cy="12" r="1"></circle>
                    <circle cx="19" cy="12" r="1"></circle>
                    <circle cx="5" cy="12" r="1"></circle>
                  </svg>
                </button>
              </div>

              <div
                v-if="!!user.you"
                class="chat__conversation-board__message__person"
              >
                <div class="chat__conversation-board__message__person__avatar">
                  <img
                    src="https://thewongandonly.com/couldbeyou.jpeg"
                    alt="You"
                  />
                </div>
                <span
                  class="chat__conversation-board__message__person__nickname"
                  >You</span
                >
              </div>
              <div
                v-if="!!user.you"
                class="chat__conversation-board__message__context"
              >
                <div class="chat__conversation-board__message__bubble">
                  <span>{{ user.you }}</span>
                </div>
              </div>
              <div
                v-if="!!user.you"
                class="chat__conversation-board__message__options"
              >
                <button
                  class="btn-icon chat__conversation-board__message__option-button option-item emoji-button"
                >
                  <svg
                    class="feather feather-smile sc-dnqmqq jxshSx"
                    xmlns="http://www.w3.org/2000/svg"
                    width="24"
                    height="24"
                    viewBox="0 0 24 24"
                    fill="none"
                    stroke="currentColor"
                    stroke-width="2"
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    aria-hidden="true"
                  >
                    <circle cx="12" cy="12" r="10"></circle>
                    <path d="M8 14s1.5 2 4 2 4-2 4-2"></path>
                    <line x1="9" y1="9" x2="9.01" y2="9"></line>
                    <line x1="15" y1="9" x2="15.01" y2="9"></line>
                  </svg>
                </button>
                <button
                  class="btn-icon chat__conversation-board__message__option-button option-item more-button"
                >
                  <svg
                    class="feather feather-more-horizontal sc-dnqmqq jxshSx"
                    xmlns="http://www.w3.org/2000/svg"
                    width="24"
                    height="24"
                    viewBox="0 0 24 24"
                    fill="none"
                    stroke="currentColor"
                    stroke-width="2"
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    aria-hidden="true"
                  >
                    <circle cx="12" cy="12" r="1"></circle>
                    <circle cx="19" cy="12" r="1"></circle>
                    <circle cx="5" cy="12" r="1"></circle>
                  </svg>
                </button>
              </div>
            </div>
          </div>

          <div class="chat__conversation-panel">
            <div class="chat__conversation-panel__container">
              <button
                class="chat__conversation-panel__button panel-item btn-icon add-file-button"
              >
                <svg
                  class="feather feather-plus sc-dnqmqq jxshSx"
                  xmlns="http://www.w3.org/2000/svg"
                  width="24"
                  height="24"
                  viewBox="0 0 24 24"
                  fill="none"
                  stroke="currentColor"
                  stroke-width="2"
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  aria-hidden="true"
                >
                  <line x1="12" y1="5" x2="12" y2="19"></line>
                  <line x1="5" y1="12" x2="19" y2="12"></line>
                </svg>
              </button>
              <button
                class="chat__conversation-panel__button panel-item btn-icon emoji-button"
              >
                <svg
                  class="feather feather-smile sc-dnqmqq jxshSx"
                  xmlns="http://www.w3.org/2000/svg"
                  width="24"
                  height="24"
                  viewBox="0 0 24 24"
                  fill="none"
                  stroke="currentColor"
                  stroke-width="2"
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  aria-hidden="true"
                >
                  <circle cx="12" cy="12" r="10"></circle>
                  <path d="M8 14s1.5 2 4 2 4-2 4-2"></path>
                  <line x1="9" y1="9" x2="9.01" y2="9"></line>
                  <line x1="15" y1="9" x2="15.01" y2="9"></line>
                </svg>
              </button>
              <input
                class="chat__conversation-panel__input panel-item"
                placeholder="Type a message..."
                v-model="question"
              />
              <button
                class="chat__conversation-panel__button panel-item btn-icon send-message-button"
                @click="onquestion"
              >
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  width="24"
                  height="24"
                  viewBox="0 0 24 24"
                  fill="none"
                  stroke="currentColor"
                  stroke-width="2"
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  aria-hidden="true"
                  data-reactid="1036"
                >
                  <line x1="22" y1="2" x2="11" y2="13"></line>
                  <polygon points="22 2 15 22 11 13 2 9 22 2"></polygon>
                </svg>
              </button>
            </div>
          </div>
        </div>
      </div>
    </transition>
  </div>
</template>
<script>
export default {
  data: () => ({
    isPanelOpen: false,
    question: "",
    convo: [
      {
        text: "Busy at the moment but circling back to you soonest! In the meantime, is there anything else I can help you with?",
      },
    ],
    answers: [
      {
        text: "Running a quick errand but I'll be back in a flash to answer your question.",
      },
      {
        Text: "Helping out with something right now, but I'll be free to chat in a jiffy.",
      },
      {
        text: "Just finishing up a task, then I'll be all yours!  Shouldn't be long.",
      },
      {
        text: "Caught up in something at the moment, but I'm almost done and will respond then.",
      },
      {
        text: "On it!  Be right back with an answer after I tie up a loose end.",
      },
      //   {
      //     text: "Busy at the moment but circling back to you soonest! In the meantime, is there anything else I can help you with?",
      //   },
      {
        text: "Wrapping something up, but your question is next on my list! I'll be back in a sec.",
      },
      {
        text: "Multitasking like a champ, but I won't forget about you!  Just a couple more minutes and I'll be here.",
      },
      {
        text: "Quick detour, but I'll be ready to answer your question in a pinch.",
      },
      {
        text: "Hold that thought!  Just gotta finish this up and I'll be back to chat in no time.",
      },
      {
        text: "That's a thought-provoking question!  I'll do some research and get back to you with a well-informed answer.",
      },
      {
        text: "That's a great inquiry!  To give you the best answer, can you tell me what aspect you're most interested in?",
      },
      {
        text: "Absolutely!  This is a complex topic, so would you prefer a general overview or something more specific?",
      },
      {
        text: "Interesting!  I'm diving into this right now and will get you a response as soon as I have something solid.",
      },
      {
        text: "That's a fantastic question!  I'm always learning, and this will be a great chance to explore this further together.  I'll get back to you with a comprehensive answer.",
      },
      {
        text: "I like where you're going with this!  Give me a sec to gather my thoughts and I'll provide a thorough response.",
      },
      {
        text: "Wow, that's a deep question!  I appreciate you challenging me.  Can you give me some time to formulate a thoughtful response?",
      },
      {
        text: "On it!  This question sparks my curiosity, and I'm eager to provide a well-researched answer.",
      },
      {
        text: "This is a fascinating topic!  I'll craft a response that does it justice, but it might take a few minutes.",
      },
      {
        text: "That's a great question!  Is there anything specific you'd like me to focus on in the answer?",
      },
      {
        text: "Absolutely!  I'm happy to tackle this question.  Can you tell me if there's a particular angle you'd like me to consider?",
      },
      {
        text: "Interesting!  This will require some deep thinking.  Can you give me some time to process and respond thoughtfully?",
      },
      {
        text: "That's a complex question, and I want to provide a well-rounded answer.  Would you prefer a breakdown into smaller parts?",
      },
      {
        text: "Absolutely!  This is a great opportunity to learn more.  I'll do some digging and get you a response that covers all the bases.",
      },
      {
        text: "On it!  I'm intrigued by this question, and I'll craft a response that considers all aspects.",
      },
      {
        text: "That's a fantastic question!  I'm always learning, and this will be a great chance to explore this further.  Can you give me some time to formulate a comprehensive answer?",
      },
      {
        text: "This is a fascinating inquiry!  I'll provide a well-researched answer, but it might take a few minutes to gather my thoughts.",
      },
      {
        text: "That's a great question!  Is there a specific area you'd like me to focus on in the answer, or are you open to a general overview?",
      },
      {
        text: "Absolutely!  I'm happy to tackle this question.  Can you tell me if there's a particular angle you'd like me to consider?",
      },
    ],
  }),
  methods: {
    closeSidebarPanel() {
      console.log("closeSidebarPanel");
      this.isPanelOpen = false;
    },
    delayedAction(fn, wait) {
      setTimeout(() => {
        fn();
      }, wait);
    },
    onquestion() {
      const q = this.question;
      if (!q) return;
      this.convo.push({ you: q });
      this.question = "";
      this.delayedAction(() => {
        const ans = this.randomIntFromInterval(0, this.answers.length - 1);
        this.convo.push(this.answers[ans]);
      }, 3500);
    },
    randomIntFromInterval(min, max) {
      // min and max included
      return Math.floor(Math.random() * (max - min + 1) + min);
    },
  },

  computed: {
    chatconvo() {
      return [...this.convo];
    },
  },
};
</script>
<style scoped>
.slide-enter-active,
.slide-leave-active {
  transition: transform 0.2s ease;
}

.slide-enter,
.slide-leave-to {
  transform: translateX(100%);
  transition: all 150ms ease-in 0s;
}

.sidebar {
  position: relative;
  right: -439px;
}
.sidebar-backdrop {
  background-color: #0b0715;
  opacity: 69%;
  width: 100vw;
  height: 68px;
  position: fixed;
  z-index: 9999999;
  cursor: pointer;
}

.sidebar-panel {
  position: fixed;
  overflow-y: auto;
  height: 100vh;
  top: 24px;
}

.--dark-theme {
  --chat-background: rgba(10, 14, 14, 0.95);
  --chat-panel-background: #131719;
  --chat-bubble-background: #14181a;
  --chat-bubble-active-background: #171a1b;
  --chat-add-button-background: #212324;
  --chat-send-button-background: #8147fc;
  --chat-text-color: #a3a3a3;
  --chat-options-svg: #a3a3a3;
}

.chat {
  background: var(--chat-background);
  max-width: 600px;
  margin: 25px auto;
  box-sizing: border-box;
  padding: 1em;
  border-radius: 12px;
  position: relative;
  overflow: hidden;
}
.chat::before {
  content: "";
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;

  z-index: -1;
}
.chat .btn-icon {
  position: relative;
  cursor: pointer;
}
.chat .btn-icon svg {
  stroke: #fff;
  fill: #fff;
  width: 50%;
  height: auto;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
.chat .chat__conversation-board {
  padding: 1em 0 2em;
  height: calc(100vh - 55px - 2em - 25px * 2 - 0.5em - 3em);
  overflow: auto;
}
.chat .chat__conversation-board__message-container.reversed {
  flex-direction: row-reverse;
}
.chat
  .chat__conversation-board__message-container.reversed
  .chat__conversation-board__message__bubble {
  position: relative;
}
.chat
  .chat__conversation-board__message-container.reversed
  .chat__conversation-board__message__bubble
  span:not(:last-child) {
  margin: 0 0 2em 0;
}
.chat
  .chat__conversation-board__message-container.reversed
  .chat__conversation-board__message__person {
  margin: 0 0 0 1.2em;
}
.chat
  .chat__conversation-board__message-container.reversed
  .chat__conversation-board__message__options {
  align-self: center;
  position: absolute;
  left: 0;
  display: none;
}
.chat .chat__conversation-board__message-container {
  position: relative;
  display: flex;
  flex-direction: row;
}
.chat
  .chat__conversation-board__message-container:hover
  .chat__conversation-board__message__options {
  display: flex;
  align-items: center;
}
.chat
  .chat__conversation-board__message-container:hover
  .option-item:not(:last-child) {
  margin: 0 0.5em 0 0;
}
.chat .chat__conversation-board__message-container:not(:last-child) {
  margin: 0 0 2em 0;
}
.chat .chat__conversation-board__message__person {
  text-align: center;
  margin: 0 1.2em 0 0;
}
.chat .chat__conversation-board__message__person__avatar {
  height: 35px;
  width: 35px;
  overflow: hidden;
  border-radius: 50%;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;

  position: relative;
}
.chat .chat__conversation-board__message__person__avatar::before {
  content: "";
  position: absolute;
  height: 100%;
  width: 100%;
}
.chat .chat__conversation-board__message__person__avatar img {
  height: 100%;
  width: auto;
}
.chat .chat__conversation-board__message__person__nickname {
  font-size: 9px;
  color: #484848;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
  display: none;
}
.chat .chat__conversation-board__message__context {
  max-width: 55%;
  align-self: flex-end;
}
.chat .chat__conversation-board__message__options {
  align-self: center;
  position: absolute;
  right: 0;
  display: none;
}
.chat .chat__conversation-board__message__options .option-item {
  border: 0;
  background: 0;
  padding: 0;
  margin: 0;
  height: 16px;
  width: 16px;
  outline: none;
}
.chat .chat__conversation-board__message__options .emoji-button svg {
  stroke: var(--chat-options-svg);
  fill: transparent;
  width: 100%;
}
.chat .chat__conversation-board__message__options .more-button svg {
  stroke: var(--chat-options-svg);
  fill: transparent;
  width: 100%;
}
.chat .chat__conversation-board__message__bubble span {
  width: -webkit-fit-content;
  width: -moz-fit-content;
  width: fit-content;
  display: inline-table;
  word-wrap: break-word;
  background: var(--chat-bubble-background);
  font-size: 13px;
  color: var(--chat-text-color);
  padding: 0.5em 0.8em;
  line-height: 1.5;
  border-radius: 6px;
  font-family: "Lato", sans-serif;
}
.chat .chat__conversation-board__message__bubble:not(:last-child) {
  margin: 0 0 0.3em;
}
.chat .chat__conversation-board__message__bubble:active {
  background: var(--chat-bubble-active-background);
}
.chat .chat__conversation-panel {
  background: var(--chat-panel-background);
  border-radius: 12px;
  padding: 0 1em;
  height: 55px;
  margin: 0.5em 0 0;
}
.chat .chat__conversation-panel__container {
  display: flex;
  flex-direction: row;
  align-items: center;
  height: 100%;
}
.chat .chat__conversation-panel__container .panel-item:not(:last-child) {
  margin: 0 1em 0 0;
}
.chat .chat__conversation-panel__button {
  background: grey;
  height: 20px;
  width: 30px;
  border: 0;
  padding: 0;
  outline: none;
  cursor: pointer;
}
.chat .chat__conversation-panel .add-file-button {
  height: 23px;
  min-width: 23px;
  width: 23px;
  background: var(--chat-add-button-background);
  border-radius: 50%;
}
.chat .chat__conversation-panel .add-file-button svg {
  width: 70%;
  stroke: #54575c;
}
.chat .chat__conversation-panel .emoji-button {
  min-width: 23px;
  width: 23px;
  height: 23px;
  background: transparent;
  border-radius: 50%;
}
.chat .chat__conversation-panel .emoji-button svg {
  width: 100%;
  fill: transparent;
  stroke: #54575c;
}
.chat .chat__conversation-panel .send-message-button {
  background: var(--chat-send-button-background);
  height: 30px;
  min-width: 30px;
  border-radius: 50%;
  transition: 0.3s ease;
}
.chat .chat__conversation-panel .send-message-button:active {
  transform: scale(0.97);
}
.chat .chat__conversation-panel .send-message-button svg {
  margin: 1px -1px;
}
.chat .chat__conversation-panel__input {
  width: 100%;
  height: 100%;
  outline: none;
  position: relative;
  color: var(--chat-text-color);
  font-size: 13px;
  background: transparent;
  border: 0;
  font-family: "Lato", sans-serif;
  resize: none;
}
.wishtochat {
  position: fixed;
  bottom: 0;
  right: 0;
  width: 50px;
  height: 42px;
  cursor: pointer;
}
.close {
  left: 388px;
  top: 8px;
  cursor: pointer;
  position: relative;
}
</style>
