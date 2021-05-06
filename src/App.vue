<template>
  <the-credit-meter :credit="credit"></the-credit-meter>
  <div>
    <base-reel
      class="base-reel"
      v-if="spinning"
      :items="items"
      :animation-class="animationClass0"
      :current-item="currentItem"
      @complete="spinComplete"
    ></base-reel>
    <base-reel
      class="base-reel"
      v-if="spinning"
      :items="items"
      :animation-class="animationClass1"
      :current-item="currentItem"
      @complete="spinComplete"
    ></base-reel>
    <base-reel
      class="base-reel"
      v-if="spinning"
      :items="items"
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
      currentItem: {
        label: 'Kitten',
        url: 'https://placekitten.com/100/100',
      },
      spinning: false,
      spinsCompleted: 0,
      items: [
        {
          label: 'Kitten',
          url: 'https://placekitten.com/100/100',
        },
        {
          label: 'Bill Murray',
          url: 'https://www.fillmurray.com/100/100',
        },
        {
          label: 'People',
          url: 'https://placeimg.com/100/100/people',
        },
        {
          label: 'Kitten',
          url: 'https://placekitten.com/100/100',
        },
        {
          label: 'Bill Murray',
          url: 'https://www.fillmurray.com/100/100',
        },
        {
          label: 'People',
          url: 'https://placeimg.com/100/100/people',
        },
        {
          label: 'Kitten',
          url: 'https://placekitten.com/100/100',
        },
        {
          label: 'Bill Murray',
          url: 'https://www.fillmurray.com/100/100',
        },
        {
          label: 'People',
          url: 'https://placeimg.com/100/100/people',
        },
        {
          label: 'Kitten',
          url: 'https://placekitten.com/100/100',
        },
        {
          label: 'Bill Murray',
          url: 'https://www.fillmurray.com/100/100',
        },
        {
          label: 'People',
          url: 'https://placeimg.com/100/100/people',
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
      for (let i = 0; i < 3; i++) {
        /*
        console.log('setting', i);
        this.currentItems[i] = this.items[
          Math.floor(Math.random() * this.items.length)
        ];
        */
      }
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
    processSpin() {
      console.log('PROCESSING SPIN');
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