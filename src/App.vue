<template>
  <div id="app">
    <VisualYear/>
  </div>
</template>

<script>
import VisualYear from './components/VisualYear.vue'

export default {
  name: 'App',
  components: {
    VisualYear
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
