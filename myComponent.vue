<template>
  <div class="full-height">
    <div class="container" v-if="!showOverlay">
      <div class="centered-element">
        <div class="content">
          <div class="inner">
            <img
              @click="showvid"
              :src="pic"
              alt="David Wong"
              class="manhole clickable"
              :class="{ ball: technoball, shake: technoshake }"
            />
            <h1>David</h1>
            <h1>thewongandonly</h1>
            <p class="iam">

              <a href="https://x.com/thewongandonly" target="_blank" rel="noopener noreferrer" style="border: none; text-decoration: none" title="contact">
        <svg width="50px" height="50px" version="1.1" id="svg5" xmlns:svg="http://www.w3.org/2000/svg" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" x="0px" y="0px" viewBox="0 0 1668.56 1221.19" style="enable-background: new 0 0 1668.56 1221.19" xml:space="preserve">
          <style type="text/css">
            .st0 {
              stroke: #ffffff;
              stroke-miterlimit: 10;
            }
            .st1 {
              fill: #ffffff;
            }
          </style>
          <g>
            <circle class="st0" cx="834.28" cy="610.6" r="481.33"></circle>
            <g id="layer1" transform="translate(52.390088,-25.058597)">
              <path id="path1009" class="st1" d="M485.39,356.79l230.07,307.62L483.94,914.52h52.11l202.7-218.98l163.77,218.98h177.32
			L836.82,589.6l215.5-232.81h-52.11L813.54,558.46L662.71,356.79H485.39z M562.02,395.17h81.46l359.72,480.97h-81.46L562.02,395.17
			z"></path>
            </g>
          </g>
        </svg>
      </a>
              

              <br />
              fullstack developer since 2000<br />
              {{ msg }} <br />
              Outside of programming, I like
              <span class="clickable" @click="setBackground('YourName.mp4')"
                >anime</span
              >,
              video
              <span class="clickable" @click="showDoki">games</span>, 
              <span class="clickable" @click="setBackground('Sorry.mp4')"
                >K-drama</span
              >,
              <span class="clickable" @click="setBackground('bowlramen.mp4')"
                >Japanese food</span
              >,
              and 
              <span class="clickable" @click="onTechno">melodic techno</span>.
            </p>
          </div>
        </div>
      </div>
    </div>

    <div class="fullheightinner" v-if="showOverlay" @click="showvid">
      <div class="myvid">
        <video autoplay muted loop id="myVideo2">
          <source :src="vid" type="video/mp4" />
        </video>
      </div>

      <div class="overlay">
        <div class="inneroverlay">
          <h1>David</h1>
          <h1>thewongandonly</h1>
        </div>
      </div>
      <div class="wanted">
        <h1>reward $31,650,848,009</h1>
        <p>In gold coin for the capture</p>
      </div>
    </div>
  </div>

  <div v-if="animateBlue" class="hero-slider curscreen"></div>
</template>

<script>
export default {
  emits: ["customChange"],
  mounted() {
    this.audio = new Audio("https://thewongandonly.com/gunshot.mp3");
    this.audiodokidoki = new Audio("https://thewongandonly.com/BGM03g.mp3");
    this.audiotechno = new Audio(
      "https://thewongandonly.com/melodic-techno.mp3"
    );
  },
  data() {
    return {
      audio: null,
      audiodokidoki: null,
      audiotechno: null,
      animateBlue: false,
      curAnimeIndex: 0,
      listUrls: [
        "url('https://thewongandonly.com/tenor.gif')",
        'url("https://thewongandonly.com/windows-8-blue-screen-of-death.jpg")',
        'url("https://thewongandonly.com/Eron-Rauch-Doki-Doki-Screen-Sculpture-1.jpg")',
        'url("https://thewongandonly.com/dokimonika.jpg")',
      ],
      vid: "wongmoving.mp4",
      showOverlay: false,
      technoed: false,
      technoball: false,
      technoshake: false,
      pic: "wongbw.png",
      msg: "I use vue.js and C# to create dockerized microservices for remarkable digital experiences.",
    };
  },
  methods: {
    onTechno() {
      this.setBackground("sea.mp4");

      if (this.technoed) return;
      this.technoed = true;
      this.audiotechno.volume = 0.3;
      this.audiotechno.play();
      this.delayedAction(() => {
        this.technoball = true;
      }, 18000);
      this.delayedAction(() => {
        this.technoball = false;
      }, 35000);
      this.delayedAction(() => {
        this.technoshake = true;
      }, 3000);
      this.delayedAction(() => {
        this.technoshake = false;
      }, 17500);
    },
    setBackground(name) {
      this.$emit("customChange", name);
    },
    playgun() {
      this.audio.volume = 0.35;
      this.audio.play();
    },
    playdoki() {
      this.audiodokidoki.volume = 0.35;
      this.audiodokidoki.play();
    },
    delayedAction(fn, wait) {
      setTimeout(() => {
        fn();
      }, wait);
    },
    nextIndex() {
      const cur = this.curAnimeIndex + 1;
      this.curAnimeIndex = cur % this.listUrls.length;
    },
    showDoki() {
      let base = 3000;

      this.delayedAction(() => {
        this.animateBlue = true;
        this.setBackground("Sorry.mp4")
      }, 10);
      for (let index = 1; index < this.listUrls.length; index++) {
        this.delayedAction(this.nextIndex, base + 5400 * (index + 1));
      }

      this.delayedAction(this.playdoki, base + 15000);

      this.delayedAction(() => {
        this.animateBlue = false;
        this.curAnimeIndex = 0;
        this.setBackground("darkcode.mp4")
      }, base + 48000);
    },
    showvid() {
      const play = !this.showOverlay;
      this.showOverlay = !this.showOverlay;
      if (play) {
        this.delayedAction(this.playgun, 400);
        this.delayedAction(this.playgun, 3900);
      } else this.setBackground("darkcode.mp4");
    },
  },
  computed: {
    backgroundurl() {
      return this.listUrls[this.curAnimeIndex];
    },
  },
};
</script>

<style scoped>
  
.st0 {
stroke: #ffffff;
stroke-miterlimit: 10;
}
.st1 {
fill: #ffffff;
}
          
.hero-slider {
  position: relative;
  height: 100vh;
  display: flex;
  background: #030303;

  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
}
.curscreen {
  background-image: v-bind(backgroundurl);
}

.myvid {
  margin-top: 110px;
}
.overlay {
  position: relative;
  width: calc(100%);
  height: 580px;
  margin: 0 auto;
  top: -581px;
  overflow: hidden;
}
.overlay:after {
  content: "";
  position: absolute;
  left: 239px;
  top: 25px;
  border-radius: 100%;
  width: 525px;
  height: 525px;
  box-shadow: 0px 0px 0px 2000px #17181b;
}
.inneroverlay {
  margin-top: 445px;
}

.fullheightinner {
  height: 100%;
  background-color: #17181b;
}

.container {
  display: grid;
  align-items: center;
  height: 600px;
  border: 0px solid #006100;
}
.iam {
  font-size: 0.9rem;
  line-height: 1.5em;
}
.manhole {
  margin-bottom: 14px;
  width: 200px;
  cursor: pointer;
}
.clickable {
  cursor: pointer;
  text-decoration: underline;
}
.full-height {
  position: absolute;
  height: 100vh;
  width: 100%;
  color: #f3e1d1;

  font-family: "Montserrat", sans-serif !important;

  display: -moz-flex;
  display: -webkit-flex;
  display: -ms-flex;
  display: flex;
  -moz-flex-direction: column;
  -webkit-flex-direction: column;
  -ms-flex-direction: column;
  flex-direction: column;
  -moz-align-items: center;
  -webkit-align-items: center;
  -ms-align-items: center;
  align-items: center;
  max-width: 100%;
  text-align: center;
}

h1 {
  font-weight: 13 !important;

  color: #ffffff;
  line-height: 1.5;
  margin: 0 0 1rem 0;
  text-transform: uppercase;
  letter-spacing: 2rem;

  font-size: 1.1rem;
  line-height: 1;
  letter-spacing: 0.42rem;
}
.wanted {
  position: relative;
  top: -555px;
}

.ball {
  animation: bounce 0.42s;
  animation-direction: alternate;
  animation-timing-function: cubic-bezier(0.5, 0.05, 1, 0.5);
  animation-iteration-count: infinite;
}
.shake {
  animation: jumpshaking 1.83s infinite;
}
@keyframes bounce {
  from {
    transform: translate3d(0, 0, 0);
  }
  to {
    transform: translate3d(0, 90px, 0);
  }
}

@keyframes jumpshaking {
  0% {
    transform: translateX(0);
  }
  25% {
    transform: translateY(-9px);
  }
  35% {
    transform: translateY(-9px) rotate(7deg);
  }
  55% {
    transform: translateY(-9px) rotate(-7deg);
  }
  65% {
    transform: translateY(-9px) rotate(7deg);
  }
  75% {
    transform: translateY(-9px) rotate(-7deg);
  }
  100% {
    transform: translateY(0) rotate(0);
  }
}
</style>
