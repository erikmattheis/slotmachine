<template>
  <div>
    <button @click="showReel = !showReel">show reel</button>
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
    transform: translateY(-1000%);
  }
  90% {
    transform: translateY(-100%);
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

.bounce-enter-from,
.bounce-leave-to {
  animation-name: example;
}

.bounce-enter-active {
  animation: bounce 3s;
}

ul {
  list-style: none;
  display: flex;
  flex-direction: column;
}
li {
  flex-grow: 1;
  display: flex;
  align-items: center;
}
</style>
