<template>
  <div class="full-height">
    <div class="container" v-if="!showOverlay">
      <div class="centered-element">
        <div class="content">
          <div class="inner">
            <img @click="showvid" :src="pic" alt="David Wong" class="manhole" />
            <h1>David</h1>
            <h1>thewongandonly</h1>
            <p class="iam">
              fullstack developer<br />
              {{ msg }} <br />
              {{ likes }}
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
        <h1>reward $1,975,000</h1>
        <p>In gold coin for the capture</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  mounted() {
    this.audio = new Audio("https://thewongandonly.com/gunshot.mp3");
  },
  data() {
    return {
      audio: null,
      vid: "wongmoving.mp4",
      showOverlay: false,
      pic: "wongbw.png",
      msg: "I use vue.js and C# to create dockerized microservices for remarkable digital experiences.",
      likes:
        "Outside of programming, I enjoy video games, K-drama, Japanese food, anime, and melodic techno.",
    };
  },
  methods: {
    playgun() {
      this.audio.volume = 0.35;
      this.audio.play();
    },
    showvid() {
      const play = !this.showOverlay;
      this.showOverlay = !this.showOverlay;
      if (play) {
        setTimeout(() => {
          this.playgun();
        }, 600);

        setTimeout(() => {
          this.playgun();
        }, 4100);
      }
    },
  },
};
</script>

<style scoped>
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
</style>
