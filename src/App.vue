<template>
  <div id="app">
    <span class="text">AVCDSDS</span>
    <HelloWorld msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue";
import { mapGetters } from 'vuex'
export default {
  name: "app",
  components: {
    HelloWorld,
  },
  computed:{
    ...mapGetters(['test'])
  },
  mounted(){
    this.$store.dispatch('setTest',10).then(()=>{
      console.log(this.test)
    })
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
