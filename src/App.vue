<template>
  <div id="app">
    <home></home>
  </div>
</template>

<script>
import Home from './components/Home'
import Single from './components/Single'

export default {
  name: 'app',
  components: {
    Home, Single
  }
}
</script>

<style>
#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #fff;
  margin-top: 20px;
}
</style>
