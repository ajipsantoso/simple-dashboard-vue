<template>
  <div id="app">
    <!-- <img alt="Vue logo" src="./assets/logo.png"> -->
    <!-- <HelloWorld msg="Welcome to Your Vue.js App"/> -->
    <dashboard/>
  </div>
</template>

<script>
import Dashboard from './page/Dashboard.vue';

export default {
  name: 'app',
  components: {
    Dashboard,
  },
};
</script>

<style>
html{
  background: rgb(238, 238, 238);
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 20px;
}
</style>
