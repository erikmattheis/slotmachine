<template>
  <div class="reel-window" :class="{ 'bounce animated': showReel }">
    <ul>
      <symbol-face
        :url="item.url"
        v-for="(item, index) in items"
        v-bind:key="index"
      >
        ></symbol-face
      >
    </ul>
  </div>
</template>

<script>
import SymbolFace from './SymbolFace';
export default {
  components: {
    SymbolFace,
  },
  data() {
    return {
      animated: false,
      currentTransitionNum: 0,
    };
  },
  props: ['items', 'currentItem', 'showReel'],
  methods: {
    animationBeforeEnter(el) {
      console.log('before eneter', el);
      el.style.transform = 'scale(0.1)';
    },
    animationEnter(el, done) {
      const delay = el.dataset.index * 300;

      console.log('on enter', el, delay);

      setTimeout(() => {
        el.style.transform = 'scale(1.0)';
        done();
      }, delay);
    },
  },
};
</script>

<style scoped>
.animated {
  animation-duration: 3s;
  animation-fill-mode: both;
}
@keyframes bounce {
  0% {
    transform: translateY(-100%);
  }
  90% {
    transform: translateY(-10%);
  }
  95% {
    transform: translateY(5%);
  }
  98% {
    transform: translateY(-2%);
  }
  100% {
    transform: translateY(-30px);
  }
}

.bounce {
  animation: bounce 5s;
}

ul {
  list-style: none;
  display: flex;
  flex-direction: column;
  padding: 0;
  margin: 0;
}
li {
  flex-grow: 1;
  display: flex;
}

.reel-window {
  width: 200px;
  height: 600px;
  clip-path: polygon(0 0, 100% 0, 100% 100%, 0% 100%);
}

.perspective-on {
  -webkit-perspective: 1000px;
  -moz-perspective: 1000px;
  perspective: 1000px; /* Setting the perspective of the contents of the stage but not the stage itself*/
}
</style>
