<template>
  credit: {{ credit }}
  <the-credit-meter :credit="credit"></the-credit-meter>

  <div>
    <base-reel
      class="base-reel"
      v-if="animationTrigger"
      :items="items0"
      :animation-class="animationClass0"
      @complete="spinComplete"
    ></base-reel>
    <base-reel
      class="base-reel"
      v-if="animationTrigger"
      :items="items1"
      :animation-class="animationClass1"
      @complete="spinComplete"
    ></base-reel>
    <base-reel
      class="base-reel"
      v-if="animationTrigger"
      :items="items2"
      :animation-class="animationClass2"
      @complete="spinComplete"
    ></base-reel>
  </div>

  <three-d-button
    @click="spin(1)"
    :disabled="spinning || broke"
  ></three-d-button>
</template>

<script>
import TheCreditMeter from './components/TheCreditMeter';
import BaseReel from './components/BaseReel';
import ThreeDButton from './components/ThreeDButton';
export default {
  data() {
    return {
      credit: 50,
      animationClass0: 'no-bounce',
      animationClass1: 'no-bounce',
      animationClass2: 'no-bounce',
      items0: [],
      items1: [],
      items2: [],
      sounds: {
        click: new Audio('/assets/audio/click.wav'),
        short: new Audio('/assets/audio/short.mp3'),
        medium: new Audio('/assets/audio/medium.mp3'),
        full: new Audio('/assets/audio/full.mp3'),
        spin: new Audio('/assets/audio/spin.wav'),
      },
      playerPlaying: false,
      spinning: false,
      animationTrigger: true,
      spinsCompleted: 0,
      payTable: {
        Prince: 1000,
        Ibis: 200,
        Archie: 100,
        Cake: 50,
        '3 Boxes': 30,
        '2 Boxes': 20,
        '1 Box': 10,
      },
      items: [
        {
          winner: true,
          type: 'wild',
          label: 'Prince',
          url: '/assets/faces/00-prince.jpg',
        },
        {
          winner: true,
          type: 'ibis',
          label: 'Ibis',
          url: '/assets/faces/01-ibis.jpg',
        },
        {
          winner: true,
          type: 'archie',
          label: 'Archie',
          url: '/assets/faces/03-archie.jpg',
        },
        {
          winner: true,
          type: 'cake',
          label: 'Cake',
          url: '/assets/faces/04-cake.jpg',
        },
        {
          winner: true,
          type: 'box',
          label: '1 Box',
          url: '/assets/faces/06-bar-1.jpg',
        },
        {
          winner: true,
          type: 'box',
          label: '2 Boxes',
          url: '/assets/faces/07-bar-2.jpg',
        },
        {
          winner: true,
          type: 'box',
          label: '3 Boxes',
          url: '/assets/faces/08-bar-3.jpg',
        },
        {
          type: 'musician',
          label: 'Elton',
          url: '/assets/faces/09-elton.jpg',
        },
        {
          type: 'musician',
          label: 'Lady Gaga',
          url: '/assets/faces/10-gaga.jpg',
        },
        {
          type: 'musician',
          label: 'The Artist',
          url: '/assets/faces/11-prince.jpg',
        },
        {
          type: 'musician',
          label: 'Beyonce',
          url: '/assets/faces/13-beyonce.jpg',
        },
      ],
    };
  },
  computed: {
    broke() {
      return this.credit < 1;
    },
  },
  components: {
    TheCreditMeter,
    BaseReel,
    ThreeDButton,
  },
  mounted() {
    this.sounds.full.addEventListener('ended', () => {
      this.spinning = false;
    });
    this.sounds.medium.addEventListener('ended', () => {
      this.spinning = false;
    });
    this.sounds.short.addEventListener('ended', () => {
      this.spinning = false;
    });
    this.shuffleItems();
  },
  methods: {
    shuffleItems() {
      this.items0 = this.shuffledItems(this.items);
      this.items1 = this.shuffledItems(this.items);
      this.items2 = this.shuffledItems(this.items);
    },
    spin(n) {
      this.shuffleItems();
      this.playerPlaying = true;
      this.spinning = true;
      this.credit -= n;
      this.animationClass0 =
        'bounce-enter-active-' + Math.floor(Math.random() * 7);
      this.animationClass1 =
        'bounce-enter-active-' + Math.floor(Math.random() * 7);
      this.animationClass2 =
        'bounce-enter-active-' + Math.floor(Math.random() * 7);
      this.animationTrigger = false;
      this.spinsCompleted = 0;
      this.sounds.click.play();
      setTimeout(
        function () {
          this.sounds.spin.play();
          this.animationTrigger = true;
        }.bind(this),
        0
      );
    },
    shuffledItems(array) {
      const arr = [];
      let randomIndex;
      for (let i = 0; i < array.length; i++) {
        randomIndex = Math.floor(Math.random() * array.length);
        arr.push(array[randomIndex]);
      }

      return arr;
    },
    processSpin() {
      const result = this.getScore();
      this.credit += result.credit;
      if (result.winner) {
        this.playWinningSound(result);
      } else {
        this.spinning = false;
      }
    },
    playWinningSound(result) {
      if (result.label === 'JACKPOT') {
        this.sounds.full.play();
        this.winner(result);
      } else if (result.winner) {
        this.sounds.medium.play();
        this.winner(result);
      } else if (result.credit) {
        this.sounds.short.play();
      }
    },
    getScore() {
      let results = [this.items0[1], this.items1[1], this.items2[1]];
      results = results.filter((o) => o.type !== 'wild');

      const firstSymbol = results[0];
      let labelsMatch =
        results.length === 0 ||
        results.every((o) => o.label === firstSymbol.label);
      let typesMatch =
        results.length === 0 ||
        results.every((o) => o.type === firstSymbol.type);
      let result;
      if (!results.length) {
        result = {
          winner: true,
          result: 'JACKPOT',
          credit: this.payTable['wild'],
        };
      } else if (labelsMatch && firstSymbol.winner) {
        result = {
          winner: true,
          result: firstSymbol.label,
          credit: this.payTable[firstSymbol.label],
        };
      } else if (typesMatch) {
        result = {
          winner: false,
          result: 'Free Spin',
          credit: 1,
        };
      } else {
        result = {
          result: null,
          credit: 0,
        };
      }
      return result;
    },
    winner() {
      console.log('WINNER');
    },
    spinComplete() {
      if (!this.playerPlaying) {
        return;
      }
      this.spinsCompleted++;
      if (this.spinsCompleted === 3) {
        this.sounds.spin.pause();
        this.processSpin();
      }
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
  margin: 0;
}

.base-reel {
  float: left;
}
</style>