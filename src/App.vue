<template>
  <div id="app">
    <ImageEditor></ImageEditor>
  </div>
</template>

<script>
import ImageEditor from "./components/ImageEditor.vue";

export default {
  name: "App",
  components: {
    ImageEditor,
  },
};
</script>
<style lang="less" scoped>
#app {
  background-color: #000;
}
</style>
