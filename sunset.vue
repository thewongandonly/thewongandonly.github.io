<template>
  <div class="full-height">
    <div class="container" v-if="!showOverlay">
      <div class="centered-element">
        <div class="content">
         
		</div>
      </div>
    </div>
  </div>
 
</template>


<script>
export default {
  emits: ["customChange"],
  mounted() {
 
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
      pic: "wong.jpg",  wanted: "wanted.jpg",
      msg: "Is your name Wong too? Would you like a landing page here at thewongandonly.com/yourlandingpage? Contact me at send.to.wong@gmail.com and we start from there...",
    };
  },
  methods: {
    setBackground(name) {
      this.$emit("customChange", name);
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
	
</style>
