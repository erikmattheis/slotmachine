
      shortWinSound: 
      credit: 50,
      animationClass0: '',
      animationClass1: '',
      animationClass2: '',
      sounds: {
        click: new Audio('/assets/audio/click.wav'),
      },
      items0: [],
      items1: [],
      items2: [],
      spinning: false,
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
      this.sounds.click.play();
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
      } else if (labelsMatch && firstSymbol.winner) {
        this.credit += this.payTable[firstSymbol.label];
        this.winner();
      } else if (typesMatch) {
        this.credit += 1;
      }
    },
    winner() {
      console.log('WINNER');
    },
    spinComplete() {
      this.spinsCompleted++;
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