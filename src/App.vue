<template>
  <div id="app">
    <Stats />
  </div>
</template>

<script>
import Stats from "./components/Stats/Stats.vue";

export default {
  name: "app",
  components: {
    Stats
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
