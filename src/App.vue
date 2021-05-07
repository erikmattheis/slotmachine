<template>
  <the-credit-meter :credit="credit"></the-credit-meter>
  <div>
    <base-reel
      class="base-reel"
      v-if="spinning"
      :items="items0"
      :animation-class="animationClass0"
      :current-item="currentItem"
      @complete="spinComplete"
    ></base-reel>
    <base-reel
      class="base-reel"
      v-if="spinning"
      :items="items1"
      :animation-class="animationClass1"
      :current-item="currentItem"
      @complete="spinComplete"
    ></base-reel>
    <base-reel
      class="base-reel"
      v-if="spinning"
      :items="items2"
      :animation-class="animationClass2"
      @complete="spinComplete"
    ></base-reel>
  </div>

  <button @click="spin(1)">Bet 1</button>
  <button @click="spin(2)">Bet 2</button>
</template>

<script>
import TheCreditMeter from './components/TheCreditMeter';
import BaseReel from './components/BaseReel';
export default {
  data() {
    return {
      credit: 50,
      animationClass0: '',
      animationClass1: '',
      animationClass2: '',
      items0: [],
      items1: [],
      items2: [],
      spinning: false,
      spinsCompleted: 0,
      payTable: [
        { Prince: 1000 },
        { Ibis: 200 },
        { Archie: 100 },
        { cake: 50 },
        { '3 Boxes': 30 },
        { '2 Boxes': 20 },
        { '1 Boxes': 10 },
      ],
      items: [
        {
          type: 'prince',
          label: 'Prince',
          url: '/assets/faces/00-prince.jpg',
        },
        {
          type: 'ibis',
          label: 'Ibis',
          url: '/assets/faces/01-ibis.jpg',
        },
        {
          type: 'archie',
          label: 'Archie',
          url: '/assets/faces/03-archie.jpg',
        },
        {
          type: 'cake',
          label: 'Cake',
          url: '/assets/faces/04-cake.jpg',
        },
        {
          type: 'box',
          label: '1 Box',
          url: '/assets/faces/06-bar-1.jpg',
        },
        {
          type: 'box',
          label: '2 Boxes',
          url: '/assets/faces/07-bar-2.jpg',
        },
        {
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
  components: {
    TheCreditMeter,
    BaseReel,
  },
  computed: {},
  methods: {
    spin(n) {
      this.credit -= n;

      this.items0 = this.shuffledItems(this.items);
      this.items1 = this.shuffledItems(this.items);
      this.items2 = this.shuffledItems(this.items);
      this.animationClass0 =
        'bounce-enter-active-' + Math.floor(Math.random() * 7);
      this.animationClass1 =
        'bounce-enter-active-' + Math.floor(Math.random() * 7);
      this.animationClass2 =
        'bounce-enter-active-' + Math.floor(Math.random() * 7);
      this.spinning = false;
      this.spinsCompleted = 0;
      setTimeout(
        function () {
          this.spinning = true;
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
      const symbol0 = this.items0[1];
      const symbol1 = this.items1[1];
      const symbol3 = this.items0[1];
      console.log(symbol0.label, symbol1.label, symbol3.label);
    },
    spinComplete() {
      this.spinsCompleted++;
      console.log('this.spinsCompleted', this.spinsCompleted);
      if (this.spinsCompleted === 3) {
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