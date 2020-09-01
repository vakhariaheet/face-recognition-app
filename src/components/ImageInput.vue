<template>
  <div class="container">
    <input
      type="text"
      name="imageInput"
      id="input"
      v-model="Submit"
      class="image-input"
      placeholder="Type image URL to recognize faces in the image"
    />
    <button class="image-submit" @click="onSubmit" id="input__btn">
      Submit
    </button>
  </div>
</template>

<script>
export default {
  props: ["url"],
  data() {
    return {
      Submit: ""
    };
  },
  methods: {
    onSubmit() {
      //  Tells App Component that url is change
      this.$emit("update", this.Submit);
    }
  }
};
</script>

<style lang="scss">
@import "./Styles/Varibles.scss";

.image {
  &-input {
    padding: 1rem;
    max-width: 700px;
    width: 100%;
    font-size: 1.5rem;
    border: none;
    background-color: $yellow-orange-color-wheel;
    border-radius: 30px 0 0 30px;
    color: #353531;
    border: 4px solid transparent;
    transition: all 0.5s ease;
    &::placeholder {
      color: $black-olive;
    }
    &:hover {
      border: 4px solid $spanish-blue;
    }
    &:active,
    &:focus {
      outline: none;
      color: $yellow-orange-color-wheel;
      background-color: $spanish-blue;
      &::placeholder {
        color: $yellow-orange-color-wheel;
      }
    }
    @media (max-width: 600px) {
      & {
        height: max-content;
        padding: 1rem;
        width: 100%;
        max-width: 100%;
        border-radius: 3rem;
        font-size: 1rem;
      }
    }
  }

  &-submit {
    border-radius: 0 3rem 3rem 0;
    height: 100%;
    border: none;
    font-size: 1.5rem;
    max-width: 200px;
    width: 100%;
    color: $black-olive;
    background-color: $yellow-orange-color-wheel;
    cursor: pointer;
    transition: all 0.5s ease;
    border-left: 2px solid $black-olive;
    @media (max-width: 600px) {
      & {
        height: max-content;
        padding: 1rem;
        width: 100%;
        max-width: 100%;
        border-radius: 3rem;
        margin: 1rem 0;
        border-color: transparent;
      }
    }
    &:hover {
      color: $yellow-orange-color-wheel;
      background-color: $spanish-blue;
    }
    &:focus {
      border: 4px solid $spanish-blue;
      outline: none;
    }
  }
}

.container {
  height: 50px;
  display: flex;
  justify-content: center;
  @media (max-width: 600px) {
    & {
      flex-direction: column;
    }
  }
}
</style>
