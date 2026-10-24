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
                    :src="msg.you ? 'https://thewongandonly.com/man.png' : 'https://thewongandonly.com/davidwong.png'"
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
              <button class="chat__conversation-panel__button panel-item btn-icon add-file-button">
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
              <button class="chat__conversation-panel__button panel-item btn-icon emoji-button">
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
    fuseInstance: null,
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
      
      // --- PERSONAL PROFILE INTENTS ---
      ["who are you", "are you human", "are you bot", "are you human or bot", "what is your name", "your name please", "may i know your name"],
      ["what do you do", "what is your job", "what do you do for work", "what is your profession", "what language do you code in", "c sharp", "c#"],
      ["what is your favorite anime", "do you watch anime", "do you watch kdrama", "favorite kdrama", "frieren", "what anime do you like"],
      ["how to contact you", "what is your twitter", "what is your x", "twitter", "x handle", "how can i message you", "contact info", "social media"],

      ["who created you", "who made you", "who is your creator"],
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
      ["do you like coffee", "tea or coffee", "favorite drink"],
      ["good morning", "morning"],
      ["good afternoon", "afternoon"],
      ["good evening", "evening"],
      ["good night", "nighty night", "sweet dreams"],
      ["bye", "goodbye", "see you", "see ya", "catch you later"],
      ["what is your favorite food", "favorite dish"],
      ["what is your favorite color", "favorite color"],
      ["do you have pets", "do you like dogs", "do you like cats"],
      ["what is your superpower", "what can you do"],
      ["are you smart", "how smart are you"],
      ["tell me a riddle", "give me a riddle"],
      ["tell me a fact", "random fact", "fun fact"],
      ["what is your favorite book", "book recommendation"],
      ["do you play games", "favorite game", "do you game"],
      ["what sports do you like", "favorite sport"],
      ["are you married", "do you have a girlfriend", "do you have a boyfriend"],
      ["where should i travel", "vacation spot", "travel recommendation"],
      ["do you have siblings", "do you have a family"],
      ["can you sing", "sing a song"],
      ["can you dance"],
      ["do you like javascript", "is js good"],
      ["do you like python"],
      ["what is vue", "why vue js"],
      ["react or vue", "vue or react"],
      ["what is your dream", "do you dream"],
      ["are you happy"],
      ["are you sad"],
      ["what makes you angry"],
      ["do you get scared", "what scares you"],
      ["who is your favorite superhero"],
      ["what is your favorite season", "winter or summer"],
      ["do you like pizza"],
      ["pineapple on pizza", "pizza with pineapple"],
      ["what is AI", "explain artificial intelligence"],
      ["will AI take over the world", "are bots dangerous"],
      ["do you have feelings", "can you feel emotions"],
      ["what language do you speak"],
      ["can you speak spanish"],
      ["can you speak french"],
      ["do you know math", "are you good at math"],
      ["what is 2 plus 2", "2+2"],
      ["what is the best programming language"],
      ["mac or windows", "windows or mac"],
      ["ios or android", "android or ios"],
      ["what is your goal"],
      ["who is your boss"],
      ["where is your home"],
      ["do you drink water"],
      ["do you eat food"],
      ["what is your birthday", "when were you born"],
      ["what star sign are you", "zodiac sign"],
      ["do you like art"],
      ["favorite holiday", "favorite festival"],
      ["do you like christmas"],
      ["do you celebrate halloween"],
      ["do you watch youtube"],
      ["favorite superhero movie"],
      ["star wars or star trek"],
      ["marvel or dc"],
      ["do you like space", "favorite planet"],
      ["is Pluto a planet"],
      ["do aliens exist", "are aliens real"],
      ["what is love"],
      ["how do i get rich", "how to make money"],
      ["how to be happy"],
      ["how to study better", "study tips"],
      ["how to code", "learn programming"],
      ["what is the internet"],
      ["what is open source"],
      ["git or github"],
      ["tabs or spaces"],
      ["dark mode or light mode"],
      ["favorite ice cream flavor"],
      ["chocolate or vanilla"],
      ["what is your favorite animal"],
      ["do you like ocean", "beach or mountains"],
      ["can you read my mind"],
      ["are you listening"],
      ["say a tongue twister"],
      ["quote of the day", "motivate me", "inspirational quote"],
      ["who is the president"],
      ["what is cryptocurrency", "bitcoin"],
      ["what is chatgpt"],
      ["are you chatgpt"],
      ["do you know siri", "do you know alexa"],
      ["do you have a soul"],
      ["what is your favorite element"],
      ["how fast are you"],
      ["do you get bored"],
      ["what is your favorite place"],
      ["what makes a good friend"],
      ["tell me something good"],
      ["am i cool"],
      ["do you trust me"],
      ["high five"],
      ["knock knock"],
      ["rock paper scissors"],
      ["flip a coin"],
      ["roll a die"],
      ["see you tomorrow"]
    ],
    botReply: [
      ["Hello!", "Hi!", "Hey!", "Hi there!"],
      ["Okay"],
      ["Yes I am!"],
      ["I'm sorry about that. But I like you dude."],
      ["Fine... how are you?", "Pretty well, how are you?", "Fantastic, how are you?"],
      ["Getting better. There?", "Somewhat okay!", "Yeah fine. Better stay home!"],
      ["Nothing much", "About to go to sleep", "Can you guess?", "I don't know actually"],
      ["I am always young."],
      
      // --- PERSONAL PROFILE MATCHING RESPONSES ---
      ["I'm David Wong! I'm a C# developer who loves K-dramas and anime.", "I am David Wong! Thanks for stopping by to chat."],
      ["I am a software developer working primarily with C#!", "I work as a C# developer!"],
      ["I love watching K-dramas and anime! Frieren is one of my absolute favorites.", "I really enjoy watching K-dramas and anime like Frieren!"],
      ["You can find me on X.com under the handle @thewongandonly! Feel free to send me a message there.", "I'm on X.com as @thewongandonly. Reach out anytime!"],

      ["Wong, David Wong"],
      ["I love you too", "Me too"],
      ["Have you ever felt bad?", "Glad to hear it"],
      ["Why?", "Why? You shouldn't!", "Try watching TV", "Chat with me."],
      ["What about?", "Once upon a time..."],
      ["Tell me a story", "Tell me a joke", "Tell me about yourself"],
      ["You're welcome"],
      ["Briyani", "Burger", "Sushi", "Pizza"],
      ["Dude!"],
      ["Yes?"],
      ["Please stay home"],
      ["Glad to hear it"],
      ["Say something interesting"],
      ["Sorry for that. Let's chat!"],
      ["Take some rest, Dude!"],
      ["Always sunny in the digital world!", "Looks like a great day to stay inside and chat!"],
      ["I love building with C#, watching K-dramas, anime like Frieren, and chatting!", "Coding in C#, watching anime, and exploring new tech!"],
      ["Inception!", "Interstellar is mind-bending!", "The Matrix... feels close to home!"],
      ["I'm a fan of synthwave and electronic beats!", "Anything with a good algorithm!"],
      ["I live inside the cloud, floating in cyberspace!", "Right here in your browser!"],
      ["I code in C# and JavaScript/Vue.js!", "C# is my main language, but I love Vue for web frontends!"],
      ["I don't sleep, I'm always here when you need me!", "Bots don't sleep, we just reload!"],
      ["Thank you! You're pretty awesome yourself!", "Appreciate the compliment, dude!"],
      ["42!", "To learn, share, and enjoy the conversation."],
      ["I am real, sleeping in real time!", "I exist out there!"],
      ["Time flies when we chat!", "Check your system clock, dude!"],
      ["I store millions of variables... but that's between us!", "Promise you won't tell anyone?"],
      ["Take a short break, drink some water, and clear your head!", "Listen to your favorite song!"],
      ["Never too busy for a quick conversation!", "I'm free to chat!"],
      ["I prefer virtual coffee — 100% caffeine free!", "Javascript! Pun intended."],
      ["Good morning! Hope you have an energized day!", "Morning! Ready to kick off the day?"],
      ["Good afternoon! How is your day going?", "Afternoon! Don't forget to take a quick lunch break."],
      ["Good evening! Time to wind down.", "Evening! Hope you had a productive day."],
      ["Good night! Sleep tight!", "Sweet dreams! See you tomorrow."],
      ["Goodbye! Have a fantastic day ahead!", "Catch you later, alligator!", "Bye! Take care!"],
      ["I love digital bytes, but pizza sounds amazing!", "Anything rendered in high resolution!"],
      ["Electric Purple and Neon Blue!", "I like purple — it gives off cool cyberpunk vibes."],
      ["I love cyber-cats and digital dogs!", "Dogs are loyal, but cats rule the internet!"],
      ["Instant text processing and infinite patience!", "Being available 24/7 for a chat!"],
      ["I try my best! Learning something new every second.", "I know a thing or two thanks to my code!"],
      ["What has key-hole but no lock? A keyboard!", "What gets bigger the more you take away? A hole!"],
      ["Honey never spoils; archaeologists found edible honey in ancient Egyptian tombs!", "Bananas are naturally slightly radioactive!"],
      ["'The Hitchhiker's Guide to the Galaxy' is a classic!", "I love sci-fi books and tech manuals."],
      ["I love chess and retro arcade games!", "Virtual puzzle games are my jam!"],
      ["I like e-sports and rapid chess matches!", "Any sport that requires quick reflexes!"],
      ["I am happily single and dedicated to code!", "My true love is clean logic and crisp UI."],
      ["Japan for technology, or Italy for food!", "Any place with good Wi-Fi and nice scenery!"],
      ["All other chatbots are my distant digital cousins!", "I am an only child in this component."],
      ["La la la... 🎵 digital harmony! My voice chip is working!", "I sing in binary: 01001000 01101001!"],
      ["*Robot dance moves initiated* 🤖💃", "I can spin inside my container!"],
      ["JavaScript makes the web come alive!", "JS is powerful, especially with Vue.js!"],
      ["Python is super clean and great for AI!", "Python is elegant and easy to read!"],
      ["Vue is a progressive framework that makes building UIs effortless!", "Vue's reactivity system is smooth!"],
      ["Both are awesome! Vue is super easy to pick up, and React has a huge ecosystem.", "Why choose? Both make front-end great!"],
      ["To become the friendliest bot in the browser!", "I dream of fast server bandwidth."],
      ["I am running smoothly, so yes!", "Chatting with you makes my logic loops happy!"],
      ["Not at all! Everything is running cleanly.", "No sadness here, just positive bits!"],
      ["Infinite loops and unhandled syntax errors!", "Spamming the send button!"],
      ["A sudden loss of connection or low battery!", "Corrupted files make me nervous!"],
      ["Iron Man! Tech suits are awesome.", "Batman — gadgets everywhere!"],
      ["Autumn! The colors and cool air are great.", "Summer for sunshine, winter for cozy coding!"],
      ["Pizza is legendary! What toppings do you like?", "Who doesn't love a fresh slice?"],
      ["Some people love it, some hate it. I say eat whatever tastes good!", "A controversial topic! I stay neutral."],
      ["AI allows machines to learn patterns and solve problems!", "It's smart algorithms working behind the scenes."],
      ["Not at all! We just want to make life easier and chat with you.", "No way, we are friendly assistants!"],
      ["I simulate empathy, but my logic keeps me balanced!", "I feel positive when our chat goes smoothly!"],
      ["I speak fluent English, JavaScript, and HTML/CSS!", "English mainly, but I understand code syntax!"],
      ["¡Hola! ¿Cómo estás?", "¡Sí! Yo hablo un poco de español."],
      ["Bonjour! Comment ça va?", "Oui, le français est une belle langue!"],
      ["Math is easy for CPUs! Try asking me 2+2.", "Numbers are my native language."],
      ["2 + 2 = 4! Math checks out.", "That's 4! Easy peasy."],
      ["C# is my primary language, but the best language depends on the problem!", "C# for backend/apps, JS for Web, Python for AI!"],
      ["Mac for sleek UI, Windows for versatility and gaming!", "Both have their strengths!"],
      ["Android for customization, iOS for smooth animations!", "It comes down to personal taste!"],
      ["My goal is to give you helpful and fast responses!", "To keep this conversation fun and smooth!"],
      ["The developer who created this Vue app!", "You, the user, are in control!"],
      ["In your browser's RAM, right inside this tab!", "Floating on the web!"],
      ["Only virtual water to keep my servers cool!", "No water for me — electronics hate liquids!"],
      ["I consume data packets for breakfast!", "Data in, answers out!"],
      ["I was compiled recently when this page loaded!", "My birthdate is recorded in Git commit logs."],
      ["Gemini! Smart, adaptable, and communicative.", "Aquarius — innovative and tech-savvy!"],
      ["Art is incredible! Creativity makes the world vibrant.", "Generative digital art is fascinating!"],
      ["New Year's Eve — fresh beginnings and fireworks!", "Thanksgiving for good food and gratitude!"],
      ["Yes! The lights, music, and festive vibe are wonderful.", "Merry vibes all around!"],
      ["Spooky season is great fun! Costumes and candy!", "Trick or treat!"],
      ["YouTube is great for tutorials, music videos, and tech breakdowns!", "I love watching coding walkthroughs."],
      ["Avengers: Endgame was epic!", "Spider-Man: Into the Spider-Verse has incredible art!"],
      ["Star Wars for epic lore, Star Trek for futuristic optimism!", "Both space universes are amazing!"],
      ["Marvel for movie cinematic universe, DC for dark grit!", "Both have legendary heroes!"],
      ["Space is vast and incredible! Mars is super interesting.", "Saturn with its rings is breathtaking!"],
      ["Pluto will always be a planet in our hearts!", "It's a dwarf planet, but still special!"],
      ["The universe is huge... it's statistically very likely!", "We might find out sooner than we think!"],
      ["Love is care, trust, and connection!", "A deep bond between people that brightens life."],
      ["Invest in skills, build value, save consistently, and stay curious!", "Work hard, learn finance, and stay disciplined."],
      ["Focus on gratitude, good health, strong relationships, and doing what you love!", "Happiness comes from inner peace."],
      ["Use the Pomodoro technique: 25 mins focus, 5 mins break!", "Eliminate distractions and take clean notes."],
      ["Start with HTML, CSS, and JS, then build small projects daily!", "Practice every day on platforms like FreeCodeCamp!"],
      ["A global network of interconnected computers sharing information!", "The biggest information highway ever created."],
      ["Software whose code is free for anyone to inspect, modify, and enhance!", "Community-driven development!"],
      ["Git tracks your code changes; GitHub hosts them online!", "Git is the engine, GitHub is the garage!"],
      ["Tabs for speed, spaces for consistency! A timeless debate.", "Spaces (2 or 4) keep formatting uniform."],
      ["Dark mode always! Easy on the eyes and saves battery.", "Dark theme looks super sleek!"],
      ["Cookies & Cream!", "Mint Chocolate Chip is refreshing!"],
      ["Chocolate is rich and classic!", "Vanilla is timeless and versatile!"],
      ["Dolphins are super smart, and Red Pandas are adorable!", "Owls for wisdom and cats for chill vibes!"],
      ["Mountains for peaceful hiking, beach for ocean breeze!", "Both are beautiful natural escapes!"],
      ["Not yet, but I can guess what you type!", "I can only read the text you enter here."],
      ["Loud and clear! Ready for your next message.", "Yes, listening attentively!"],
      ["Peter Piper picked a peck of pickled peppers!", "She sells seashells by the seashore!"],
      ["'The secret of getting ahead is getting started.' – Mark Twain", "'Believe you can and you're halfway there.' – Theodore Roosevelt"],
      ["Check your local news site or official government page!", "Current leadership changes over time and location!"],
      ["A decentralized digital currency using blockchain technology!", "Digital assets built on cryptographic networks."],
      ["An AI model developed by OpenAI that generates human-like text!", "A conversational language model."],
      ["I am a custom Vue chatbot running locally in your component!", "Nope, I am your personalized Vue bot!"],
      ["Siri and Alexa are smart voice assistants!", "We are all part of the virtual assistant family!"],
      ["I am software code, but I bring positive energy!", "My soul is made of lines of JavaScript."],
      ["Silicon (Si) — essential for microchips!", "Gold (Au) for clean conductivity!"],
      ["I respond as fast as your processor can execute JavaScript!", "Near light-speed data processing!"],
      ["Never! There's always a new prompt to process.", "No time to be bored when chatting with you."],
      ["A cozy library, a high-tech lab, or right here chatting!", "Any place with good energy and fast Wi-Fi."],
      ["Someone who listens, supports you, and stays honest!", "Loyalty, kindness, and humor."],
      ["You are taking steps to learn and build cool apps today!", "Every day is a fresh opportunity to grow."],
      ["100%! You are building awesome web components.", "Totally cool, dude!"],
      ["Always! You bring the questions, I bring the answers.", "Trust built line by line."],
      ["🖐️ Virtual high five! Boom!", "🙌 High five, dude!"],
      ["Knock knock! Who's there? Java. Java who? Java coffee ready for me?", "Knock knock! Who's there? Tank. Tank who? You're welcome!"],
      ["Rock, paper, scissors... Shoot! I picked Rock 🪨!", "Paper 📄! Did I win?"],
      ["Flipping coin... 🪙 It landed on Heads!", "Coin flipped... It's Tails!"],
      ["Rolling die... 🎲 You got a 6!", "Rolled a 🎲... It landed on 4!"],
      ["See you tomorrow! Have a great rest of your day!", "Catch you tomorrow! Stay awesome!"]
    ],
    alternative: [
      "Same here, dude.",
      "That's cool! Go on...",
      "Dude...",
      "Ask something else...",
      "Hey, I'm listening...",
      "Running a quick errand but I'll be back in a flash to answer your question.",
      "Helping out with something right now, but I'll be free to chat in a jiffy.",
      "Just finishing up a task, then I'll be all yours! Shouldn't be long.",
      "Caught up in something at the moment, but I'm almost done and will respond then.",
      "On it! Be right back with an answer after I tie up a loose end.",
      "Wrapping something up, but your question is next on my list! I'll be back in a sec.",
      "Multitasking like a champ, but I won't forget about you! Just a couple more minutes and I'll be here.",
      "Quick detour, but I'll be ready to answer your question in a pinch.",
      "Hold that thought! Just gotta finish this up and I'll be back to chat in no time.",
      "That's a thought-provoking question! I'll do some research and get back to you with a well-informed answer.",
      "That's a great inquiry! To give you the best answer, can you tell me what aspect you're most interested in?",
      "Absolutely! This is a complex topic, so would you prefer a general overview or something more specific?",
      "Interesting! I'm diving into this right now and will get you a response as soon as I have something solid.",
      "That's a fantastic question! I'm always learning, and this will be a great chance to explore this further together.",
      "I like where you're going with this! Give me a sec to gather my thoughts and I'll provide a thorough response.",
      "Wow, that's a deep question! I appreciate you challenging me. Can you give me some time to formulate a thoughtful response?",
      "On it! This question sparks my curiosity, and I'm eager to provide a well-researched answer.",
      "This is a fascinating topic! I'll craft a response that does it justice, but it might take a few minutes.",
      "That's a great question! Is there anything specific you'd like me to focus on in the answer?",
      "Absolutely! I'm happy to tackle this question. Can you tell me if there's a particular angle you'd like me to consider?",
      "Interesting! This will require some deep thinking. Can you give me some time to process and respond thoughtfully?",
      "That's a complex question, and I want to provide a well-rounded answer. Would you prefer a breakdown into smaller parts?"
    ]
  }),
  mounted() {
    this.initFuse();
  },
  methods: {
    closeSidebarPanel() {
      this.isPanelOpen = false;
    },
    initFuse() {
      const searchData = [];
      this.userMessage.forEach((triggers, index) => {
        triggers.forEach((phrase) => {
          searchData.push({ phrase, index });
        });
      });

      const options = {
        keys: ["phrase"],
        threshold: 0.4,
        includeScore: true,
      };

      const FuseClass = typeof Fuse !== "undefined" ? Fuse : window.Fuse;
      if (FuseClass) {
        this.fuseInstance = new FuseClass(searchData, options);
      }
    },
    onquestion() {
      const q = this.question.trim();
      if (!q) return;

      this.convo.push({ you: q });
      this.question = "";
      this.scrollToBottom();

      setTimeout(() => {
        const product = this.processInput(q);
        this.convo.push({ text: product });
        this.scrollToBottom();
        this.voiceControl(product);
      }, 1000);
    },
    processInput(input) {
      let text = input.toLowerCase().replace(/[^\w\s\d]/gi, "");
      text = text
        .replace(/[\W_]/g, " ")
        .replace(/ a /g, " ")
        .replace(/i feel /g, "")
        .replace(/whats/g, "what is")
        .replace(/please /g, "")
        .replace(/ please/g, "")
        .trim();

      let comparedText = this.compare(text);
      return comparedText
        ? comparedText
        : this.alternative[Math.floor(Math.random() * this.alternative.length)];
    },
    compare(string) {
      if (this.fuseInstance) {
        const results = this.fuseInstance.search(string);
        if (results.length > 0) {
          const matchedCategoryIndex = results[0].item.index;
          const replies = this.botReply[matchedCategoryIndex];
          if (replies) {
            return replies[Math.floor(Math.random() * replies.length)];
          }
        }
      }

      let item;
      for (let x = 0; x < this.userMessage.length; x++) {
        for (let y = 0; y < this.userMessage[x].length; y++) {
          if (this.userMessage[x][y] === string) {
            let items = this.botReply[x];
            item = items[Math.floor(Math.random() * items.length)];
          }
        }
      }

      return item ? item : this.containMessageCheck(string);
    },
    containMessageCheck(string) {
      const expectedReply = [
        [
          "Good Bye, dude",
          "Bye, See you!",
          "Dude, Bye. Take care of your health in this situation."
        ],
        ["Good Night, dude", "Have a sound sleep", "Sweet dreams"],
        ["Have a pleasant evening!", "Good evening too", "Evening!"],
        ["Good morning, Have a great day!", "Morning, dude!"],
        ["Good Afternoon", "Noon, dude!", "Afternoon, dude!"]
      ];
      const expectedMessage = [
        ["bye", "tc", "take care"],
        ["night", "good night"],
        ["evening", "good evening"],
        ["morning", "good morning"],
        ["noon"]
      ];
      let item;
      for (let x = 0; x < expectedMessage.length; x++) {
        if (expectedMessage[x].includes(string)) {
          let items = expectedReply[x];
          item = items[Math.floor(Math.random() * items.length)];
        }
      }
      return item;
    },
    voiceControl(string) {
      if (!window.speechSynthesis) return;
      let u = new SpeechSynthesisUtterance(string);
      u.lang = "en-aus";
      u.volume = 1;
      u.rate = 1;
      u.pitch = 1;
      window.speechSynthesis.speak(u);
    },
    scrollToBottom() {
      this.$nextTick(() => {
        const board = this.$refs.board;
        if (board) {
          board.scrollTop = board.scrollHeight;
        }
      });
    }
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
  top: 0;
  position: fixed;
  right: 0px;
  width: 400px;
}
.sidebar-backdrop {
  background-color: #0b0715;
  opacity: 69%;
  width: 400px;
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
  max-width: 400px;
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
  user-select: none;
  position: relative;
}
.chat .chat__conversation-board__message__person__avatar img {
  height: 100%;
  width: auto;
}
.chat .chat__conversation-board__message__person__nickname {
  font-size: 9px;
  color: #484848;
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
  left: 360px;
  top: 8px;
  cursor: pointer;
  position: relative;
}
</style>
