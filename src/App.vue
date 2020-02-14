<template>
  <div id="app">
    <MyMap />
  </div>
</template>

<script>
import MyMap from './components/MyMap.vue'

export default {
  name: 'App',
  components: {
    MyMap,
  },
}
</script>

<style>
html,
body {
  height: 100%;
  margin: 0;
  padding: 0;
}

#app {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100%;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
