<template>
  <div class="slider-container">
    <div class="category-title" :style="{ color: themeColor }">
      {{ category }}
    </div>
    <input
      type="range"
      class="slider"
      :style="sliderStyle"
      @input="changeFire"
      v-model="sliderValue"
      min="0"
      max="100"
      step="1"
    />
    <div class="describe-bar">{{ describe }}</div>
  </div>
</template>
<script>
export default {
  name: "slider",
  props: {
    category: {
      type: String,
      validator: (value) => {
        return ["brightness", "contrast"].includes(value);
      },
    },
    themeColor: {
      // need to pass hex color
      type: String,
    },
    describe: {
      type: String,
    },
  },
  data() {
    return {
      sliderValue: 50,
    };
  },
  computed: {
    sliderStyle() {
      let rate = this.sliderValue,
        rgbColorArray = this.hexToRgb(this.themeColor),
        lightColor = `rgba(${rgbColorArray[0]},${rgbColorArray[1]},${rgbColorArray[2]},0.25)`;
      return {
        "-webkit-appearance": "none",
        background: `linear-gradient(
          90deg,
           ${this.themeColor} 0 ${rate}%,
           ${lightColor} ${rate}% 100%
         )`,
        "--slider-color": this.themeColor,
      };
    },
  },
  methods: {
    changeFire() {
      this.$emit("sliderChange", {
        category: this.category,
        val: this.sliderValue,
      });
    },

    hexToRgb(hex) {
      return hex
        .replace(
          /^#?([a-f\d])([a-f\d])([a-f\d])$/i,
          (m, r, g, b) => "#" + r + r + g + g + b + b
        )
        .substring(1)
        .match(/.{2}/g)
        .map((x) => parseInt(x, 16));
    },
  },
};
</script>
<style lang="less" scoped>
@describe-text-color: #42506b;
.slider-container {
  text-align: center;
  background-color: @white;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  border-radius: 5px;
  padding: 6px 20px 0px;
  .category-title {
    font-family: @font-Medium;
    font-size: @font-size-big;
    line-height: 25px;
    text-transform: capitalize;
  }

  .slider {
    margin-top: 15px;
    margin-bottom: 13px;
    width: 100%;
    border-radius: 5px;
    height: 5px;
  }
  .describe-bar {
    font-family: @font-Regular;
    font-size: @font-size-medium;
    line-height: 25px;
    color: @describe-text-color;
  }
}

.thumb-style {
  width: 15px;
  height: 15px;
  border-radius: 15px;
  background-color: var(--slider-color);
  border: 3px solid @white;
  box-sizing: content-box;
  margin-top: -7.5px;
}

.track-style {
  height: 5px;
  border-radius: 5px;
}

.slider-container {
  /*Chrome*/
  input[type="range"]::-webkit-slider-runnable-track {
    -webkit-appearance: none;
    .track-style();
  }

  input[type="range"]::-webkit-slider-thumb {
    -webkit-appearance: none;
    .thumb-style ();
  }

  // /** FF*/
  input[type="range"]::-moz-range-thumb {
    .thumb-style ();
  }
  input[type="range"]::-moz-range-track {
    .track-style();
  }
}
</style>
