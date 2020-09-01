<template>
  <div class="mainContainer">
    <div class="container" v-if="faces">
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
      <img :src="imgUrl" alt=" " v-if="imgUrl" class="img" id="img" />
      <img
        :src="imgUrl"
        alt=" "
        v-if="imgUrl"
        class="img"
        id="img1"
        @load="OnImg"
      />
    </div>
    <p id="err">{{ errMsg }}</p>
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
      this.img.height = event.target.height;
      this.img.width = event.target.width;
    }
  }
};
</script>

<style lang="scss" scoped>
@import "./Varibles.scss";
img {
  position: relative;
  z-index: 0;
  border: 4px solid $spanish-blue;
  border-radius: 3rem;
}
#img {
  max-width: 500px;
  width: 100%;
  height: 100%;
  transition: all 0.5s ease;

  &:hover {
    filter: drop-shadow(2px 4px 6px black);
  }
}
.faces {
  border: 3px solid $majorelle-blue;
  border-radius: 5px;
  position: absolute;
  z-index: 10;
}
.container {
  position: absolute;
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
#err {
  font-size: 2rem;
  color: $yellow-orange-color-wheel;
}
</style>
