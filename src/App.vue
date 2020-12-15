<template>
  <div id="app">
    <Home/>
  </div>
</template>

<script>
import Home from './components/Home.vue'

export default {
  name: 'App',
  components: {
    Home
  }
}
</script>

<style>
  body {
    margin: 0;

    font-family: Helvetica,Arial,sans-serif;
  }

  ul {
    margin: 0;
    padding: 0;
    list-style-type: none;
  }
</style>
