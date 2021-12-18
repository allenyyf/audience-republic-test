<template>
  <div :style="setStyle()" class="avatar-container">
    <img :src="imageUrl" alt="" />
  </div>
</template>
<script>
export default {
  name: "avatar",
  props: {
    width: {
      type: Number,
      default: 60,
    },
    height: {
      type: Number,
      default: 60,
    },
    isCircle: {
      type: Boolean,
      default: true,
    },
    imageUrl: {
      type: String,
      default: "",
    },
  },
  methods: {
    setStyle() {
      let styleObj = {
        width: this.width + "px",
        height: this.height + "px",
      };
      if (this.isCircle) {
        styleObj["borderRadius"] = this.width + "px";
      }
      return styleObj;
    },
  },
};
</script>
<style lang="less" scoped>
.avatar-container {
  background-color: #e5e5e5;
  border: 2px solid #fff;
  overflow: hidden;
  img {
    max-width: 100%;
    max-height: 100%;
    object-fit: cover;
  }
}
</style>
