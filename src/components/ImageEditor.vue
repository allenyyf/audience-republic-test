<template>
  <div class="image-editor">
    <Header></Header>
    <div class="content-container">
      <Slider
        category="brightness"
        :themeColor="brightnessThemeColor"
        describe="Slide to adjust image brightness! ☀️"
        @sliderChange="updateSliderValue"
        :disabled="!uploadImage"
      ></Slider>
      <Slider
        class="contrast-slider"
        category="contrast"
        :themeColor="contrastThemeColor"
        describe="Slide to adjust image contrast! 🌗"
        @sliderChange="updateSliderValue"
        :disabled="!uploadImage"
      ></Slider>
      <Uploader
        class="uploader"
        :filter="filter"
        @uploadImage="uploadImageFire()"
      ></Uploader>
    </div>
  </div>
</template>
<script>
import Slider from "@/components/Slider.vue";
import Header from "@/components/Header.vue";
import Uploader from "@/components/Uploader.vue";
export default {
  name: "image-editor",
  components: {
    Header,
    Slider,
    Uploader,
  },
  data() {
    return {
      brightnessThemeColor: "#25A95B",
      contrastThemeColor: "#4A90E2",
      filter: {
        brightness: 0,
        contrast: 0,
      },
      uploadImage: false,
    };
  },
  methods: {
    updateSliderValue(event) {
      let { category, val } = event;
      this.filter[category] = val;
    },
    uploadImageFire() {
      this.uploadImage = true;
    },
  },
};
</script>
<style lang="less" scoped>
.image-editor {
  margin: auto;
  max-width: 375px;
  .content-container {
    padding: 17px 20px 20px;
    background-color: @white;
    .contrast-slider {
      margin-top: 7px;
    }
    .uploader {
      margin-top: 29px;
    }
  }
}
</style>
