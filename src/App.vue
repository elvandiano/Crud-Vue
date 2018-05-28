<template>
  <div id="app" class="container">
    <router-view/>
  </div>
</template>
<script>
  export default {
    name: "App"
  }
</script>
<style>
  #app {
    font-family: 'Xingkai TC', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    color: #2c3e50;
    font-weight: bold;
  }
  body {
    margin: 0;
    padding: 0;
    border: 0;
    background-image: url("assets/bgrn.jpg");
  }
</style>
