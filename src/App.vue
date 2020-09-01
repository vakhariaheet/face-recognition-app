<template>
  <div id="app">
    <div class="mode">
      <div class="change-mode-btn">
        <div class="change-mode-btn__toggle night-toggle"></div>
      </div>
    </div>

    <h1 class="heading">Face Recognition App</h1>
    <vueParticles
      color="#ffffff"
      :particleOpacity="0.7"
      linesColor="#ffffff"
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
      errMsg: ""
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
    }
  }
};
</script>

<style lang="scss">
@import "./components/Varibles.scss";
body {
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
}

.change-mode-btn {
  background: #212121;
  height: 50px;
  width: 100px;
  border-radius: 100px;
  display: flex;
  align-items: center;
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
  font-family: "Josefin Slab", serif;
}
.particles {
  position: fixed;
  height: 100%;
  width: 100vw;
  z-index: 1;
}
</style>
