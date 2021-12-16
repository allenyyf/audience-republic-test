<template>
  <div class="header">
    <h3 class="title-bar">Brightness & Contrast Developer Test</h3>
    <h4 class="time-bar">01 Jun, 2018 – 31 Dec, 2019</h4>
  </div>
</template>
<style lang="less" scoped>
.header {
  height: 70px;
  text-align: center;
  background-color: @purple;
  color: #fff;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
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
</style>
