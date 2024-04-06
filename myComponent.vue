<template>
  <div class="full-height">
    <div class="container">
      <div class="centered-element">
        <div class="content">
          <div class="inner">
            <img
              src="https://thewongandonly.com/Wongcircle.png"
              height="200"
              alt="David Wong"
              style="margin-bottom: 10px"
            />

            <h1>David Wong</h1>
            <h1>thewongandonly.com</h1>

            <p style="font-size: 0.9rem">
              fullstack developer<br />
              {{ msg }} <br />
              {{ likes }}
            </p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      msg: "I use vue.js and C# to create dockerized microservices for remarkable digital experiences.",
      likes:
        "Outside of programming, I enjoy video games, K-drama, Japanese food, anime, and melodic techno.",
    };
  },
};
</script>

<style scoped>
.container {
  display: grid;
  align-items: center;
  height: 600px;
  border: 0px solid #006100;
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
  -moz-transition: -moz-transform 0.325s ease-in-out,
    -moz-filter 0.325s ease-in-out, opacity 0.325s ease-in-out;
  -webkit-transition: -webkit-transform 0.325s ease-in-out,
    -webkit-filter 0.325s ease-in-out, opacity 0.325s ease-in-out;
  -ms-transition: -ms-transform 0.325s ease-in-out,
    -ms-filter 0.325s ease-in-out, opacity 0.325s ease-in-out;
  transition: transform 0.325s ease-in-out, filter 0.325s ease-in-out,
    opacity 0.325s ease-in-out;
  background-image: -moz-radial-gradient(
    rgba(0, 0, 0, 0.25) 25%,
    rgba(0, 0, 0, 0) 55%
  );
  background-image: -webkit-radial-gradient(
    rgba(0, 0, 0, 0.25) 25%,
    rgba(0, 0, 0, 0) 55%
  );
  background-image: -ms-radial-gradient(
    rgba(0, 0, 0, 0.25) 25%,
    rgba(0, 0, 0, 0) 55%
  );
  background-image: radial-gradient(
    rgba(0, 0, 0, 0.25) 25%,
    rgba(0, 0, 0, 0) 55%
  );
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
