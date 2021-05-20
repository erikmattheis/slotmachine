<template>
  <section id="game-wrapper">
    <div id="game">
      <the-credit-meter :credit="credit" class="credit"></the-credit-meter>

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
    </div>
  </section>
</template>

<script>
import TheCreditMeter from './TheCreditMeter';
import BaseReel from './BaseReel';
import ThreeDButton from './ThreeDButton';
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
    processSpin() {
      const result = this.getScore();
      this.credit += result.credit;

      this.playWinningSoundIfWinner(result);
    },
    playWinningSoundIfWinner(result) {
      if (result.label === 'JACKPOT') {
        this.sounds.full.play();
        this.winner(result);
        console.log('jackpot!');
      } else if (result.winner) {
        console.log('big winner!');
        this.sounds.medium.play();
        this.winner(result);
      } else if (result.credit) {
        console.log('free spin');
        this.sounds.short.play();
      } else {
        console.log('nothing');
        this.spinning = false;
      }
    },
    getScore() {
      let results = [this.items0[1], this.items1[1], this.items2[1]];
      results = results.filter((o) => o.type !== 'wild');
      console.log('length after wild', results.length);
      const firstSymbol = results[0];
      let labelsMatch =
        results.length === 0 ||
        results.every((o) => o.label === firstSymbol.label);
      let typesMatch = results.every((o) => o.type === firstSymbol.type);
      let result;
      if (!results.length) {
        result = {
          winner: true,
          label: 'JACKPOT',
          credit: this.payTable['wild'],
        };
      } else if (labelsMatch && firstSymbol.winner) {
        result = {
          winner: true,
          label: firstSymbol.label,
          credit: this.payTable[firstSymbol.label],
        };
      } else if (typesMatch) {
        result = {
          winner: false,
          label: 'Free Spin',
          type: firstSymbol.type,
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

<style scoped>
#game-wrapper {
  width: 1130px;
  height: 2446px;
}
#game {
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
.base-reel {
  float: left;
}
</style>