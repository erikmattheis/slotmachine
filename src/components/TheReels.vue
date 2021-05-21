<template>
  <div class="reels-container">
    <div class="reels">
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
    <div class="reels-overlay"></div>
  </div>
</template>

<script>
import BaseReel from './BaseReel';

export default {
  props: ['animationTrigger'],
  data() {
    return {
      credit: 50,
      animationClass0: 'no-bounce',
      animationClass1: 'no-bounce',
      animationClass2: 'no-bounce',
      items0: [],
      items1: [],
      items2: [],
      playerPlaying: false,
      spinning: false,
      spinsCompleted: 0,
      payTable: {
        Prince: 500,
        Ibis: 200,
        Archie: 100,
        Cake: 50,
        '3 Boxes': 30,
        '2 Boxes': 20,
        '1 Box': 10,
        boxes: 5,
        musicians: 2,
      },
      items: [
        {
          winner: true,
          type: 'wild',
          label: 'Prince',
          url: '/assets/faces/00-prince.png',
        },
        {
          winner: true,
          type: 'ibis',
          label: 'Ibis',
          url: '/assets/faces/01-ibis.png',
        },
        {
          winner: true,
          type: 'archie',
          label: 'Archie',
          url: '/assets/faces/03-archie.png',
        },
        {
          winner: true,
          type: 'cake',
          label: 'Cake',
          url: '/assets/faces/04-cake.png',
        },
        {
          winner: true,
          type: 'box',
          label: '1 Box',
          url: '/assets/faces/06-bar-1.png',
        },
        {
          winner: true,
          type: 'box',
          label: '2 Boxes',
          url: '/assets/faces/07-bar-2.png',
        },
        {
          winner: true,
          type: 'box',
          label: '3 Boxes',
          url: '/assets/faces/08-bar-3.png',
        },
        {
          type: 'musician',
          label: 'Elton',
          url: '/assets/faces/09-beyonce.png',
        },
        {
          type: 'musician',
          label: 'Lady Gaga',
          url: '/assets/faces/10-bowie.png',
        },
        {
          type: 'musician',
          label: 'Lady Gaga',
          url: '/assets/faces/11-freddie.png',
        },
        {
          type: 'musician',
          label: 'The Artist',
          url: '/assets/faces/12-gaga.png',
        },
      ],
    };
  },
  components: {
    BaseReel,
  },
  watch: {
    animationTrigger: function (newValue) {
      console.log('trigger changed', newValue);
      if (newValue) {
        this.spin();
      }
    },
  },
  methods: {
    shuffleItems() {
      this.items0 = this.shuffledItems(this.items);
      this.items1 = this.shuffledItems(this.items);
      this.items2 = this.shuffledItems(this.items);
    },
    spin() {
      this.shuffleItems();
      this.playerPlaying = true;
      this.spinning = true;
      this.animationClass0 =
        'bounce-enter-active-' + Math.floor(Math.random() * 7);
      this.animationClass1 =
        'bounce-enter-active-' + Math.floor(Math.random() * 7);
      this.animationClass2 =
        'bounce-enter-active-' + Math.floor(Math.random() * 7);
      this.spinsCompleted = 0;
    },
    shuffledItems(array) {
      const arr = [];
      let randomIndex;
      for (let i = 0; i < array.length; i++) {
        randomIndex = Math.floor(Math.random() * array.length);
        if (randomIndex === 0) {
          randomIndex = this.reduceProbability(0.8, randomIndex);
        } else if (randomIndex === 1) {
          randomIndex = this.reduceProbability(0.6, randomIndex);
        } else if (randomIndex === 2) {
          randomIndex = this.reduceProbability(0.4, randomIndex);
        } else if (randomIndex === 3) {
          randomIndex = this.reduceProbability(0.2, randomIndex);
        }

        arr.push(array[randomIndex]);
      }

      return arr;
    },
    reduceProbability(f, randomIndex) {
      let i;
      if (Math.random() < f) {
        i = Math.floor(Math.random() * this.items.length);
      } else {
        i = randomIndex;
      }
      return i;
    },
    processSpin() {},

    winner() {
      console.log('WINNER');
    },
    spinComplete() {
      if (!this.playerPlaying) {
        return;
      }
      this.spinsCompleted++;
      if (this.spinsCompleted === 3) {
        const results = [this.items0[1], this.items1[1], this.items2[1]];
        this.$emit[('spins-complete', results)];
      }
    },
  },
};
</script>

<style scoped>
#game-wrapper {
  width: 665px;
  height: 426px;
}
#game {
  background-attachment: scroll;
  background-image: url(/assets/img/game-bg.jpg);
  background-size: cover;
  position: relative;
}
.credit {
  width: 100%;
  text-align: center;
  position: absolute;
  top: 500px;
}
.reels-conteiner {
  position: relative;
  width: 665px;
  height: 426px;
}
.reels {
  background-attachment: scroll;
  background-image: url(/assets/img/reels-bg.png);
  background-size: cover;
  width: 665px;
  height: 426px;
}
.reels-overlay {
  background-attachment: scroll;
  background-image: url(/assets/img/reels-overlay.png);
  background-size: cover;
  position: absolute;
  z-index: 100;
  left: 0;
  top: 0;
  width: 665px;
  height: 426px;
}
.base-reel {
  float: left;
}

@media (-webkit-min-device-pixel-ratio: 2) {
  #game-wrapper,
  #game,
  .reels-conteiner,
  .reels,
  .reels-overlay {
    max-width: 100vw;
  }
}
button {
  color: black;
  text-decoration: none;
  font: bold 18px Arial;
  position: relative;
  display: inline-block;
  margin-right: 15px;
  padding: 15px;
  border-radius: 85px;
  width: 85px;
  height: 85px;
  outline: none;
  transition: all 0.2s ease-in-out;
  background: #a6e9f7;
  box-shadow: 0 8px 0 #529dad, 0 0 3px rgba(0, 0, 0, 0.2), 0 20px 20px #eee;
}
button span.outer {
  text-align: center;
  width: 100%;
  display: block;
  position: relative;
  top: 50%;
  transform: translateY(-50%);
}
button span.outer span.top {
  display: block;
  padding-bottom: 4px;
}
button span.outer span.bottom {
  border-top: 1px solid black;
  padding-top: 4px;
  display: block;
  text-transform: uppercase;
  line-height: 12px;
  font-size: 60%;
}
button:hover {
  background: #9cc62b;
  box-shadow: none;
  transform: translate3D(0, 8px, 0);
  background: #66cbe1;
  box-shadow: none;
}
button:disabled {
  opacity: 0.5;
}
img {
  width: 200px;
  height: 275px;
}
</style>