<template>
  <div class="margin center">
    <img
      class="logo"
      src="./assets/logo.png"
      alt="medical marijuanas logo"
    >
    <router-view/>
  </div>
</template>

<script>

export default {
  name: 'App'
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.logo {
  width: 500px;
  min-width: 250px;
  display: flex;
  flex: 1 1 0;
}
.margin {
  margin: auto;
  width: 60%;
}
.center {
  display: flex;
  flex-flow: column;
  align-items: center;
}
</style>
