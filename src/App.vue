<template>
  <div id="app">
    <div id="header">YouTube Downloader</div>
    <ScrollList />
  </div>
</template>

<script>
import ScrollList from "./components/ScrollList.vue";

export default {
  name: "App",
  components: {
    ScrollList
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
}

#header {
  width: 100vw;
  background: linear-gradient(90deg, rgb(253, 55, 5), rgb(253, 130, 16));
  text-align: center;
  padding: 10px 0px 10px 0px;
  color: white;
  font-family: Alata;
  font-size: 1.3em;
}
</style>
