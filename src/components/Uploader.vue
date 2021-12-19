<template>
  <div class="uploader-container">
    <div class="image-container flex-container">
      <canvas
        ref="canvas"
        class="canvas"
        :width="canvasWidth"
        :height="canvasHeight"
      ></canvas>
      <div class="tips-text-bar" v-if="!drawedImage">
        Please click upload button to upload image
      </div>
      <!-- <img src="" alt="" /> -->
    </div>
    <div class="panel-container">
      <div class="name-bar">
        <span class="label">NAME</span>
        <span class="content">{{ fileName }}</span>
      </div>
      <div class="uploader-button" @change="uploadImage">
        <label for="upload" class="upload">
          <span class="triangle"></span>
          <span class="text">UPLOAD</span>
        </label>
        <!-- name="upload" -->
        <input
          id="upload"
          type="file"
          accept=".png, .jpg, .jpeg"
          style="display: none"
        />
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: "uploader",
  props: {
    filter: {
      type: Object,
    },
  },
  data() {
    return {
      uploadedImage: null,
      canvasContext: null,
      drawedImage: null,
      canvasWidth: 335,
      canvasHeight: 212,
      originImageData: null,
      matchSize: null,
      fileName: "File name will display here",
    };
  },
  watch: {
    filter: {
      handler: function (newValue) {
        // draw the unchanged image
        this.drawImage(this.drawedImage);

        // let imageData = this.getImageData();
        this.applyFilter(newValue);

        // for (let key in newValue) {
        //   switch (key) {
        //     case "brightness": {
        //       if (newValue[key]) {
        //         this.applyBrightness(imageData.data, newValue[key]);
        //       }
        //       break;
        //     }

        //     case "contrast": {
        //       if (newValue[key]) {
        //         this.applyContrast(imageData.data, newValue[key]);
        //       }
        //       break;
        //     }
        //   }
        // }
        // this.deawImageByImageData(imageData);
      },
      deep: true,
    },
  },
  mounted() {
    this.initCanvas();
  },
  methods: {
    initCanvas() {
      let canvasElement = this.$refs["canvas"];
      if (canvasElement) {
        this.canvasContext = canvasElement.getContext("2d");
      }
    },

    uploadImage(e) {
      let image = e.target.files[0];
      if (image) {
        this.fileName = image.name;
        this.image = image;
        this.clearCanvas();
        this.drawImageOnCanvas(image);
      }
    },

    drawImageOnCanvas(imageFile) {
      var img = new Image();
      img.src = URL.createObjectURL(imageFile);
      img.addEventListener("load", () => {
        this.drawImage(img);
        this.drawedImage = img;
        if (this.filter) {
          this.applyFilter(this.filter);
        }
        this.$emit("uploadImage");
      });
    },

    drawImage(img) {
      let imageSize = {
        width: img.width,
        height: img.height,
      };
      let matchSize = this.matchCanvasSize(imageSize);
      this.matchSize = matchSize;
      this.canvasContext.drawImage(
        img,
        0,
        0,
        matchSize.width,
        matchSize.height
      );
    },

    matchCanvasSize(imageSize) {
      //scale the imageSize to make sure the width fill the canvas container
      let { width, height } = imageSize;
      let ratio = width / this.canvasWidth;
      return {
        width: this.canvasWidth,
        height: height / ratio,
      };
    },

    getImageData() {
      return this.canvasContext.getImageData(
        0,
        0,
        this.matchSize.width,
        this.matchSize.height
      );
    },

    clearCanvas() {
      this.canvasContext.clearRect(0, 0, this.canvasWidth, this.canvasHeight);
    },

    applyFilter(newValue) {
      let imageData = this.getImageData();

      for (let key in newValue) {
        switch (key) {
          case "brightness": {
            if (newValue[key]) {
              this.applyBrightness(imageData.data, newValue[key]);
            }
            break;
          }

          case "contrast": {
            if (newValue[key]) {
              this.applyContrast(imageData.data, newValue[key]);
            }
            break;
          }
        }
      }
      this.deawImageByImageData(imageData);
    },

    applyBrightness(data, brightness) {
      for (let i = 0; i < data.length; i += 4) {
        data[i] += 255 * (brightness / 100);
        data[i + 1] += 255 * (brightness / 100);
        data[i + 2] += 255 * (brightness / 100);
      }
    },

    truncateColor(value) {
      if (value < 0) {
        value = 0;
      } else if (value > 255) {
        value = 255;
      }

      return value;
    },

    applyContrast(data, contrast) {
      let factor = (259.0 * (contrast + 255.0)) / (255.0 * (259.0 - contrast));

      for (let i = 0; i < data.length; i += 4) {
        data[i] = this.truncateColor(factor * (data[i] - 128.0) + 128.0);
        data[i + 1] = this.truncateColor(
          factor * (data[i + 1] - 128.0) + 128.0
        );
        data[i + 2] = this.truncateColor(
          factor * (data[i + 2] - 128.0) + 128.0
        );
      }
    },

    deawImageByImageData(imageData) {
      this.canvasContext.putImageData(imageData, 0, 0);
    },
  },
};
</script>
<style lang="less" scoped>
.button-text {
  height: 31px;
  line-height: 31px;
  font-size: @font-size-small;
}

.label-button(@color,@width) {
  .button-text();
  box-sizing: border-box;
  border-radius: @border-radius 0 0 @border-radius;
  border: 1px solid @border-color;
  text-align: center;
  display: inline-block;
  width: @width;
  color: @color;
  background-color: @button-background-color;
}
.uploader-container {
  // height: 268px;
  border-radius: 5px;
  border: 1px solid @border-color;
  box-sizing: border-box;
  position: relative;
  .image-container {
    height: 212px;
    .canvas {
      position: absolute;
      top: -1px;
      left: -1px;
      border-radius: 5px 5px 0 0;
    }
    .tips-text-bar {
      color: @button-font-color;
    }
  }

  .panel-container {
    padding: 18px 8px 7px;
    display: flex;
    justify-content: space-between;
    box-sizing: border-box;
    font-family: @font-Medium;

    .name-bar {
      display: flex;
      letter-spacing: 1.1px;
      .label {
        .label-button(@button-font-color,60px);
      }
      .content {
        box-sizing: border-box;
        .button-text();
        border: 1px solid @border-color;
        border-left: 0;
        padding: 0 5px;
        border-radius: 0 @border-radius @border-radius 0;
        width: 147px;
        color: @green;
        text-overflow: ellipsis;
        overflow: hidden;
        white-space: nowrap;
      }
    }
    .uploader-button {
      display: flex;
      align-items: center;
      .upload {
        cursor: pointer;
        .label-button(@blue,86.6px);
        border-radius: @border-radius;
        display: flex;
        align-items: center;
        .triangle {
          margin-left: 9px;
          width: 0;
          height: 0;
          border-left: 4.92px solid transparent;
          border-right: 4.92px solid transparent;
          border-bottom: 8.3px solid rgba(74, 144, 226, 0.78);
        }

        .text {
          width: 53px;
          letter-spacing: 0.55px;
          margin: 0 7.61px;
        }
      }
    }
  }
}
</style>
