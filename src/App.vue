<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <BaseSelect/>
  </div>
</template>

<script>
import BaseSelect from './components/BaseSelect.vue'

export default {
  name: 'App',
  components: {
    // eslint-disable-next-line vue/no-unused-components,
    BaseSelect
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
