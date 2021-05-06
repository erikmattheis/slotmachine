<template>
  <div class="reel-window">
    <transition appear name="bounce" @after-enter="spinComplete">
      <ul v-if="spinning">
        <symbol-face
          :url="item.url"
          v-for="(item, index) in items"
          v-bind:key="index"
        >
          ></symbol-face
        >
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
    return {};
  },
  props: ['items', 'currentItem', 'spinning'],
  methods: {
    spinComplete() {
      this.$emit('complete', '');
      console.log('complete');
    },
  },
};
</script>

<style scoped>
.animated {
  animation-duration: 1s;
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

.bounce-enter-active {
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
