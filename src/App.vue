<template>
  <the-credit-meter :credit="credit"></the-credit-meter>
  <div>
    <base-reel
      v-if="spinning"
      :items="items"
      :current-item="currentItem"
      :spinning="spinning"
      @complete="spinComplete"
    ></base-reel>
    <base-reel
      v-if="spinning"
      :items="items"
      :current-item="currentItem"
      :spinning="spinning"
      @complete="spinComplete"
    ></base-reel>
    <base-reel
      v-if="spinning"
      :items="items"
      :current-item="currentItem"
      :spinning="spinning"
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
</style>