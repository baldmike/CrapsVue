<template>
  <div id="app">
    <CrapsGame/>
  </div>
</template>

<script>
import CrapsGame from './components/CrapsGame.vue'

export default {
  name: 'app',
  components: {
    CrapsGame
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
