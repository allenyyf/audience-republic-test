<template>
  <div class="image-editor">
    <Header></Header>
    <div class="content-container">
      <Slider
        category="brightness"
        :themeColor="brightnessThemeColor"
        describe="Slide to adjust image brightness! ☀️"
        @sliderChange="updateSliderValue"
      ></Slider>
      <Slider
        class="contrast-slider"
        category="contrast"
        :themeColor="contrastThemeColor"
        describe="Slide to adjust image contrast! 🌗"
        @sliderChange="updateSliderValue"
      ></Slider>
    </div>
  </div>
</template>
<script>
import Slider from "@/components/Slider.vue";
import Header from "@/components/Header.vue";
export default {
  name: "image-editor",
  components: {
    Header,
    Slider,
  },
  data() {
    return {
      brightnessThemeColor: "#25A95B",
      contrastThemeColor: "#4A90E2",
    };
  },
  methods: {
    updateSliderValue(val) {
      console.log(val);
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
  }
}
</style>
