<template>
  <div>
    <the-splash v-if="showSplash"></the-splash>
    <the-birdhouse-animation v-if="showBirdhouse"></the-birdhouse-animation>
    <the-game v-if="showGame"></the-game>
  </div>
</template>

<script>
import TheSplash from './components/TheSplash';
import TheBirdhouseAnimation from './components/TheBirdhouseAnimation';
import TheGame from './components/TheGame';

export default {
  data() {
    return {
      showSplash: true,
      showBirdhouse: false,
      showGame: false,
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
      this.showSplash = false;
      this.showBirdhouse = true;
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
  background: url('/assets/img/wood-tile.jpg');
  margin: 0;
}
</style>