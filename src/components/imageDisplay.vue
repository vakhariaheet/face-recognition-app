<template>
  <div class="mainContainer">
    <div class="image-display__container" v-if="faces">
      <div
        class="faces"
        v-for="face in faces"
        v-bind:key="face"
        :style="{
          width: `${(face.face_rectangle.width * 100) / img.width}%`,
          height: `${(face.face_rectangle.height * 100) / img.height}%`,
          top: `${(face.face_rectangle.top * 100) / img.height}%`,
          left: `${(face.face_rectangle.left * 100) / img.width}%`
        }"
      ></div>
      <img
        :src="imgUrl"
        alt=" "
        v-if="imgUrl"
        id="input__img"
        class="input_img"
      />
      <!--  To know real height and width of url image-->
      <img :src="imgUrl" alt=" " v-if="imgUrl" id="img1" @load="OnImg" />
    </div>
    <p id="err" class="err_msg">{{ errMsg }}</p>
  </div>
</template>

<script>
export default {
  props: ["imgUrl", "faces", "errMsg"],
  data() {
    return {
      img: {
        height: "",
        width: ""
      }
    };
  },
  methods: {
    OnImg(event) {
      // Stores URL Image height and width
      this.img.height = event.target.height;
      this.img.width = event.target.width;
    }
  }
};
</script>

<style lang="scss" scoped>
@import "./Styles/Varibles.scss";
img {
  position: relative;
  z-index: 0;
  border-radius: 3rem;
  max-width: 500px;
  width: 100%;
  height: 100%;
  transition: all 0.5s ease;

  &:hover {
    filter: drop-shadow(2px 4px 6px black);
  }
}
.input_img {
  border: 4px solid $spanish-blue;
}
.faces {
  border: 3px solid $black-olive;
  border-radius: 5px;
  position: absolute;
  z-index: 10;
}
.image-display__container {
  position: absolute;
  padding: 1rem;
}
.mainContainer {
  margin-top: 3rem;
  height: 50%;
  width: max-content;
  display: flex;
  justify-content: center;
}
#img1 {
  display: none;
}
.err_msg {
  transition: all 0.5s ease-out;
  font-size: 2rem;
  color: $yellow-orange-color-wheel;
}
</style>
