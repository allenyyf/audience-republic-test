<template>
  <div class="header">
    <div class="text-container">
      <h3 class="title-bar">Brightness & Contrast Developer Test</h3>
      <h4 class="time-bar">01 Jun, 2018 – 31 Dec, 2019</h4>
    </div>
    <div class="image-container">
      <img class="cover-image" :src="coverImage" alt="" />
      <div class="linear-bg"></div>
      <Avatar class="avatar" :imageUrl="avatarImage"></Avatar>
    </div>
  </div>
</template>
<script>
import Avatar from "@/components/Avatar";
export default {
  components: { Avatar },
  data() {
    return {
      coverImage: require("@/assets/images/cover-image.png"),
      avatarImage: require("@/assets/images/avatar.png"),
    };
  },
};
</script>
<style lang="less" scoped>
.header {
  background-color: @purple;
  .text-container {
    text-align: center;
    color: #fff;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    height: 70px;
    .title-bar,
    .time-bar {
      margin: 0;
    }
    .title-bar {
      font-size: @font-size-normal;
      font-family: @font-Medium;
      line-height: 25px;
    }
    .time-bar {
      font-size: @font-size-medium;
      font-family: @font-Regular;
      line-height: 20px;
    }
  }
  .image-container {
    display: flex;
    flex-direction: column;
    // justify-content: center;
    align-items: center;
    height: 195px;
    position: relative;
    .cover-image {
      width: 100%;
      height: 100%;
      object-fit: cover;
    }
    .linear-bg {
      position: absolute;
      bottom: 0;
      height: 133px;
      width: 100%;
      opacity: 0.3;
      background: linear-gradient(180deg, rgba(0, 0, 0, 0), rgba(0, 0, 0, 1));
    }
    .avatar {
      position: absolute;
      bottom: -10px;
    }
  }
}
</style>
