<template>
  <div id="app">
    <MarsRovers msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
import MarsRovers from './components/MarsRovers.vue'

export default {
  name: 'App',
  components: {
    MarsRovers
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
