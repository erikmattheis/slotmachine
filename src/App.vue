<template>
  <main>
    <the-splash v-if="showSplash" @click="playBirdhouse"></the-splash>
    <the-birdhouse-animation v-if="showBirdhouse"></the-birdhouse-animation>
    <the-game v-if="showGame"></the-game>
  </main>
</template>

<script>
import TheSplash from './components/TheSplash';
import TheBirdhouseAnimation from './components/TheBirdhouseAnimation';
import TheGame from './components/TheGame';

export default {
  data() {
    return {
      showSplash: false,
      showBirdhouse: false,
      showGame: true,
    };
  },
  provide: ['playBirdhouse'],
  components: {
    TheSplash,
    TheGame,
    TheBirdhouseAnimation,
  },
  methods: {
    playBirdhouse() {
      console.log('play birdhouse');
      this.showSplash = false;
      this.showBirdhouse = false;
      this.showGame = true;
    },
    playGame() {
      this.showBirdhouse = false;
      this.showGame = true;
    },
  },
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
  background-color: #9663c4;
  /* background-image: url('/assets/img/wood-tile.jpg'); */
  margin: 0;
}

main {
  width: 100%;
  height: 100vh;
  text-align: center;
  margin: auto;
}
</style>