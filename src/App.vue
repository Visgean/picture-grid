<template>
  <div id="app">
    <div class="container">
      <PhotoGrid/>
    </div>
  </div>
</template>

<script>
import PhotoGrid from './components/PhotoGrid.vue'

export default {
  name: 'App',
  components: {
    PhotoGrid
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
