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
          <div class="chat__conversation-board" ref="board">
            <div
              class="chat__conversation-board__message-container"
              v-for="(msg, index) in convo"
              :key="index"
              :class="{ reversed: !!msg.you }"
            >
              <div class="chat__conversation-board__message__person">
                <div class="chat__conversation-board__message__person__avatar">
                  <img
                    :src="msg.you ? 'https://thewongandonly.com/couldbeyou.jpeg' : 'https://thewongandonly.com/wongbw.png'"
                    height="44"
                    :alt="msg.you ? 'You' : 'David Wong'"
                  />
                </div>
                <span class="chat__conversation-board__message__person__nickname">
                  {{ msg.you ? 'You' : 'David Wong' }}
                </span>
              </div>
              <div class="chat__conversation-board__message__context">
                <div class="chat__conversation-board__message__bubble">
                  <span>{{ msg.text || msg.you }}</span>
                </div>
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
                @keyup.enter="onquestion"
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
    userMessage: [
      ["hi", "hey", "hello"],
      ["sure", "yes", "no"],
      ["are you genious", "are you nerd", "are you intelligent"],
      ["i hate you", "i dont like you"],
      ["how are you", "how is life", "how are things", "how are you doing"],
      ["how is corona", "how is covid 19", "how is covid19 situation"],
      ["what are you doing", "what is going on", "what is up"],
      ["how old are you"],
      ["who are you", "are you human", "are you bot", "are you human or bot"],
      ["who created you", "who made you", "who is your creator"],
      [
        "your name please",
        "your name",
        "may i know your name",
        "what is your name",
        "what call yourself"
      ],
      ["i love you"],
      ["happy", "good", "fun", "wonderful", "fantastic", "cool", "very good"],
      ["bad", "bored", "tired"],
      ["help me", "tell me story", "tell me joke"],
      ["ah", "ok", "okay", "nice", "welcome"],
      ["thanks", "thank you"],
      ["what should i eat today"],
      ["bro"],
      ["what", "why", "how", "where", "when"],
      ["corona", "covid19", "coronavirus"],
      ["you are funny"],
      ["i dont know"],
      ["boring"],
      ["im tired"],

      // --- ADDED NEW QUESTION INTENTS ---
      ["how is the weather", "what is the weather like", "is it raining"],
      ["what are your hobbies", "what do you do for fun", "what are your interests"],
      ["what is your favorite movie", "recommend a movie", "what film should i watch"],
      ["what music do you like", "favorite song", "favorite artist"],
      ["where do you live", "where are you located", "where are you from"],
      ["can you code", "what tech stack do you use", "what programming language"],
      ["do you sleep", "do you get tired", "are you sleeping"],
      ["you are cool", "you are awesome", "you are great", "nice job"],
      ["what is the meaning of life", "secret of life"],
      ["are you real", "are you living", "are you alive"],
      ["what day is it", "what time is it"],
      ["tell me a secret", "do you have a secret"],
      ["what should i do", "give me advice", "i am confused"],
      ["are you busy", "are you free"],
      ["do you like coffee", "tea or coffee", "favorite drink"]
    ],
    botReply: [
      ["Hello!", "Hi!", "Hey!", "Hi there!"],
      ["Okay"],
      ["Yes I am!"],
      ["I'm sorry about that. But I like you dude."],
      [
        "Fine... how are you?",
        "Pretty well, how are you?",
        "Fantastic, how are you?"
      ],
      ["Getting better. There?", "Somewhat okay!", "Yeah fine. Better stay home!"],
      [
        "Nothing much",
        "About to go to sleep",
        "Can you guess?",
        "I don't know actually"
      ],
      ["I am always young."],
      ["I am just a bot", "I am a bot. What are you?"],
      ["Sabitha Kuppusamy"],
      ["I am nameless", "I don't have a name"],
      ["I love you too", "Me too"],
      ["Have you ever felt bad?", "Glad to hear it"],
      ["Why?", "Why? You shouldn't!", "Try watching TV", "Chat with me."],
      ["What about?", "Once upon a time..."],
      ["Tell me a story", "Tell me a joke", "Tell me about yourself"],
      ["You're welcome"],
      ["Briyani", "Burger", "Sushi", "Pizza"],
      ["Dude!"],
