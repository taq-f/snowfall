<template>
  <div id="app">
    <button @click="toggleSnow">toggle</button>
    <HelloWorld msg="Hello, World!!"/>
    <snow-fall :snowing="snowing"/>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import SnowFall from './components/SnowFall.vue'

export default {
  name: 'app',
  components: {
    HelloWorld,
    SnowFall
  },
  data() {
    return {
      snowing: true
    }
  },
  methods: {
    toggleSnow() {
      this.snowing = !this.snowing
    }
  }
}
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
