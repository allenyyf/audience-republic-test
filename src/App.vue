<template>
  <div id="app">
    <Header></Header>
  </div>
</template>

<script>
import Header from "./components/Header.vue";

export default {
  name: "App",
  components: {
    Header,
  },
};
</script>

<style>
#app {
  max-width: 375px;
}
</style>
