<template>
  <div>
    <button @click="showReel = !showReel">show reel</button>
    <div class="reel-window perspective-on">
      <transition name="bounce">
        <ul v-if="showReel">
          <symbol-face
            :url="item.url"
            :key="index"
            v-for="(item, index) in items"
          ></symbol-face>
        </ul>
      </transition>
    </div>
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
      currentTransitionNum: 0,
      showReel: true,
    };
  },
  props: ['items', 'currentItem'],
  computed: {},
  methods: {
    nextTransition() {
      this.currentTransitionNum++;
      if (this.currentTransitionNum > 2) {
        this.currentTransitionNum = -1;
      } else {
        this.currentTransition = 't' + this.currentTransitionNum;
      }
    },
  },
};
</script>

<style scoped>
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
    transform: translateY(0);
  }
}

.bounce-enter-active {
  animation: bounce 3s;
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
