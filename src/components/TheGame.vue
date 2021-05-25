<template>
  <base-reel :items="items" animation-class="bounce-enter-active-0"></base-reel>
  <section id="game-wrapper" v-if="false">
    <div id="game">
      <the-credit-meter :credit="credit" class="credit"></the-credit-meter>
      <the-reels
        :animation-trigger="animationTrigger"
        @spins-complete="processSpin"
      ></the-reels>
      <three-d-button @click="spin(1)" :disabled="broke"></three-d-button>
    </div>
  </section>
</template>

<script>
import BaseReel from './BaseReel';
import TheCreditMeter from './TheCreditMeter';
import TheReels from './TheReels';
import ThreeDButton from './ThreeDButton';
export default {
  data() {
    return {
      credit: 50,
      sounds: {
        click: new Audio('/assets/audio/click.wav'),
        short: new Audio('/assets/audio/short.mp3'),
        medium: new Audio('/assets/audio/medium.mp3'),
        full: new Audio('/assets/audio/full.mp3'),
        spin: new Audio('/assets/audio/spin.wav'),
      },
      playerPlaying: false,
      spinning: false,
      spinsCompleted: 0,
      animationTrigger: true,
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
    BaseReel,
    TheCreditMeter,
    TheReels,
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
  },
  methods: {
    spin(n) {
      this.playerPlaying = true;
      this.spinning = true;
      this.credit -= n;
      this.animationTrigger = false;
      this.sounds.click.play();
      setTimeout(
        function () {
          this.sounds.spin.play();
          this.animationTrigger = true;
        }.bind(this),
        1000
      );
    },
    processSpin(results) {
      if (this.playerPlaying) {
        const result = this.getScore(results);
        this.credit += result.credit;
        this.sounds.spin.pause();
        this.playWinningSoundIfWinner(result);
      }
    },
    playWinningSoundIfWinner(result) {
      if (result.label === 'JACKPOT') {
        this.sounds.full.play();
        this.winner(result);
      } else if (result.winner) {
        this.sounds.medium.play();
        this.winner(result);
      } else if (result.credit) {
        this.sounds.short.play();
      } else {
        this.spinning = false;
      }
    },
    getScore(results) {
      results = results.filter((o) => o.type !== 'wild');
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
  },
};
</script>

<style scoped>
.base-reel {
  float: left;
  width: 210px;
}
#game-wrapper {
  width: 665px;
  height: 426px;
  margin: auto;
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