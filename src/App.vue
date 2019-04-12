<template>
  <div id="app">
    <span class="text">AVCDSDS</span>
    <HelloWorld msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue";

export default {
  name: "app",
  components: {
    HelloWorld
  },
  mounted(){
    this.$store.dispatch('setTest',1)
  }
};
document.addEventListener("DOMContentLoaded", () => {
  let fontSize = window.innerWidth / 10;
  fontSize = fontSize > 50 ? 50 : fontSize;
  const html = document.querySelector("html");
  html.style.fontSize = fontSize + "px";
});
</script>

<style lang="scss">
  @import "./assets/styles/global.scss";
  // #app {
  //   font-family: "Avenir", Helvetica, Arial, sans-serif;
  //   -webkit-font-smoothing: antialiased;
  //   -moz-osx-font-smoothing: grayscale;
  //   text-align: center;
  //   color: #2c3e50;
  //   margin-top: 60px;
  // }
  
  .text {
    font-family: "Days One";
    font-size: px2rem(20);
    color: rebeccapurple;
  }
</style>
