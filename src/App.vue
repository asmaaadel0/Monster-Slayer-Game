<template>
  <the-game></the-game>
</template>

<script>
import TheGame from './components/TheGame.vue'

export default {
  name: 'App',
  components: {
    TheGame
  }
}
</script>

<style>
* {
  box-sizing: border-box;
}

html {
  font-family: sans-serif;
}

body {
  margin: 0;
}
</style>
