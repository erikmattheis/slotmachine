<template>
  <the-credit-meter :credit="credit"></the-credit-meter>
  <div>
    <base-reel :items="items" :current-item="currentItem"></base-reel>
  </div>
  <!--
  <button @click="spin(1)">Bet 1</button>
  <button @click="spin(2)">Bet 2</button>
  -->
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
        url: 'https://placekitten.com/200/200',
      },
      items: [
        {
          label: 'Kitten',
          url: 'https://placekitten.com/200/200',
        },
        {
          label: 'Bill Murray',
          url: 'https://www.fillmurray.com/200/200',
        },
        {
          label: 'People',
          url: 'https://placeimg.com/200/200/people',
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
      for (let i = 0; i < 3; i++) {
        console.log('setting', i);
        this.currentItems[i] = this.items[
          Math.floor(Math.random() * this.items.length)
        ];
      }

      console.log('spin', n);
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

.reel {
  display: block;
  float: left;
}
</style>