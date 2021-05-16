<template>
  <the-game></the-game>
</template>

<script>
import TheGame from './components/TheGame';

export default {
  data() {
    return {};
  },
  components: {
    TheGame,
  },
  methods: {},
};
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Overpass+Mono:wght@700&display=swap');

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