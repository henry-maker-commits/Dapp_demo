<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <myConnect msg="Dapp Demo"/>
  </div>
</template>

<script>
import myConnect from './components/Connect.vue'

export default {
  name: 'App',
  components: {
    myConnect
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
