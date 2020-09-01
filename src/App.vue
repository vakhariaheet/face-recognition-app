<template>
  <div id="app">
    <div class="mode">
      <div class="change-mode-btn">
        <div
          class="change-mode-btn__toggle night-toggle move"
          @click="onModeChange"
        ></div>
      </div>
    </div>

    <h1 class="heading" id="heading">Face Recognition App</h1>
    <vueParticles
      color="#2B50AA"
      :particleOpacity="0.7"
      linesColor="#2B50AA"
      :particlesNumber="100"
      shapeType="circle"
      :particleSize="5"
      :linesWidth="2"
      :lineLinked="true"
      :lineOpacity="0.4"
      :linesDistance="150"
      :moveSpeed="3"
      :hoverEffect="true"
      hoverMode="repulse"
      :clickEffect="true"
      clickMode="push"
      class="particles"
    />
    <imageInput :url="url" @update="onURLChange" class="imageInput" />
    <imageDisplay
      :imgUrl="url"
      :faces="faces"
      class="imageDisplay"
      :errMsg="errMsg"
    />
  </div>
</template>

<script>
import ImageInput from "@/components/ImageInput.vue";
import imageDisplay from "@/components/imageDisplay.vue";
import vueParticles from "@/components/vue-particles/vueParticles.vue";
export default {
  name: "App",
  components: {
    ImageInput,
    imageDisplay,
    vueParticles
  },
  data() {
    return {
      url: "",
      img: "",
      faces: [],
      errMsg: "",
      isDayMode: false
    };
  },
  methods: {
    onURLChange(newURL) {
      this.url = newURL;
      fetch(
        `https://faceplusplus-faceplusplus.p.rapidapi.com/facepp/v3/detect?image_url=${newURL}`,
        {
          method: "POST",
          headers: {
            "x-rapidapi-host": "faceplusplus-faceplusplus.p.rapidapi.com",
            "x-rapidapi-key":
              "59d0b2c3c0msh86e340ae9c71bb8p13d7cbjsn0c90670232b6",
            "content-type": "application/x-www-form-urlencoded"
          },
          body: {}
        }
      )
        .then(response => {
          return response.json();
        })
        .then(faces => {
          this.faces = faces.faces;
          this.errMsg = "";
          switch (faces.error_message) {
            case "MISSING_ARGUMENTS: image_url, image_file, image_base64":
              this.errMsg = "Plz Enter Url";
              break;
            case "INVALID_IMAGE_SIZE: image_url":
              this.errMsg = "Image is too large or small";
              break;
            case "IMAGE_ERROR_UNSUPPORTED_FORMAT: image_url":
              this.errMsg = "Invalid Image Format";
              break;
            case "INVALID_IMAGE_URL":
              this.errMsg = "INVALID IMAGE URL";
              break;
            default:
              this.errMsg = faces.error_message;
              break;
          }
        })
        .catch(err => {
          console.log(err);
        });
    },
    onModeChange(event) {
      const btn = document.getElementById("input__btn");
      const input = document.getElementById("input");
      const heading = document.getElementById("heading");
      const image = document.getElementById("input__img");
      const body = document.getElementById("body");
      const favicon = document.getElementById("favicon");
      const errMsg = document.getElementById("err");

      event.target.classList.toggle("move");
      heading.classList.toggle("day-mode__heading");
      input.classList.toggle("day-mode__input");
      body.classList.toggle("day-mode__background");
      btn.classList.toggle("day-mode__btn");
      event.path[1].classList.toggle("day-mode__toogle--container");
      errMsg.classList.toggle("day-mode__err-msg");
      this.isDayMode = !this.isDayMode;
      if (image) {
        image.classList.toggle("day-mode__image");
      }
      if (this.isDayMode) {
        favicon.href = "favicon-light.png";
      } else {
        favicon.href = "favicon-dark.png";
      }
    }
  }
};
</script>

<style lang="scss">
@import "./components/Varibles.scss";
@import "./components/day-mode.styles.scss";
.body {
  background: linear-gradient(to right, #141e30, #243b55);
}
#app {
  text-align: center;
  font-size: 10px;
  height: 100vh;
  width: 100vw;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  padding: 1rem;
}
.imageInput {
  width: 100%;
}
*:not(.particles) {
  z-index: 2;
}
.heading {
  font-size: 3rem;
  margin: 3rem 1rem;
  color: $flame;
  font-family: "Arima Madurai", cursive;
  transition: all 0.5s ease-out;
}

.change-mode-btn {
  background: $sapphire;
  height: 50px;
  width: 100px;
  border-radius: 100px;
  display: flex;
  align-items: center;
  transition: all 1.5s ease-out;
  &__toggle {
    height: 40px;
    width: 40px;
    margin: 5px;
    border-radius: 50px;
    cursor: pointer;
    transition: all 1s ease-out;
  }
}
.mode {
  position: absolute;
  top: 5px;
  right: 5px;
}
.move {
  margin-left: 3.2rem;
}
.night-toggle {
  background: #fff;
}
ˇ* {
  margin: 0;
  box-sizing: border-box;
  padding: 0;
}
.particles {
  position: fixed;
  height: 100%;
  width: 100vw;
  z-index: 1;
}
</style>
