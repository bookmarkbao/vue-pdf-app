<template>
  <div id="app">
      <Viewer></Viewer>
      <!-- <HelloWorld msg="Welcome to Your Vue.js App"/> -->
  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
import Viewer from './components/ViewerAsync.vue'

export default {
  name: 'app',
  components: {
    // HelloWorld,
    Viewer
  }
}
</script>

<style>
* {
  padding: 0;
  margin: 0;
}
html,body{
  width:100%;
  height: 100%;
  overflow: hidden;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}


</style>
