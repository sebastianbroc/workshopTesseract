<template>
  <MainView/>
</template>

<script>
import MainView from './components/MainView.vue'

export default {
  name: 'App',
  components: {
    MainView
  }
}
</script>
<style>
body {
  background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.5), rgba(0, 0, 0, 1)),url("@/assets/bg.jpg");
  height: 100vh;
  background-repeat: no-repeat;
  background-size: cover;
}
</style>
