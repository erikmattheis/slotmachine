<template>
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

  <button @click="spin(1)" :disabled="spinning || broke">Bet 1</button>
  <button @click="spin(2)" :disabled="spinning || broke">Bet 2</button>
</template>

<script>
import TheCreditMeter from './components/TheCreditMeter';
import BaseReel from './components/BaseReel';
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
          label: 'Bowie',
          url: '/assets/faces/12-bowie.jpg',
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
      let results = [this.items0[1], this.items1[1], this.items2[1]];
      results = results.filter((o) => o.type !== 'wild');

      const firstSymbol = results[0];
      let labelsMatch =
        results.length === 0 ||
        results.every((o) => o.label === firstSymbol.label);
      let typesMatch =
        results.length === 0 ||
        results.every((o) => o.type === firstSymbol.type);

      if (!results.length) {
        this.credit += this.payTable['wild'];
        this.winner();
        this.sounds.full.play();
      } else if (labelsMatch && firstSymbol.winner) {
        this.credit += this.payTable[firstSymbol.label];
        this.winner();
        this.sounds.medium.play();
      } else if (typesMatch) {
        this.credit += 1;
        this.sounds.short.play();
      } else {
        this.spinning = false;
      }
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