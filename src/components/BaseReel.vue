<template>
  <div class="reel-window">
    <div class="offset">
      <transition
        appear
        name="bounce"
        :enter-active-class="animationClass"
        @after-enter="spinComplete"
      >
        <ul>
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
  props: ['items', 'animationClass'],
  methods: {
    spinComplete() {
      this.$emit('complete', '');
    },
  },
};
</script>

<style scoped>
@keyframes bounce {
  0% {
    transform: translateY(-100%);
  }
  80% {
    transform: translateY(0);
  }
  92% {
    transform: translateY(-3px);
  }
  100% {
    transform: translateY(0);
  }
}

.no-bounce {
  animation: 0.2s;
}
.bounce-enter-active-0 {
  animation: bounce 2s;
}
.bounce-enter-active-1 {
  animation: bounce 2.05s;
}
.bounce-enter-active-2 {
  animation: bounce 2.1s;
}
.bounce-enter-active-3 {
  animation: bounce 2.15s;
}
.bounce-enter-active-4 {
  animation: bounce 2.2s;
}
.bounce-enter-active-5 {
  animation: bounce 2.25s;
}
.bounce-enter-active-6 {
  animation: bounce 2.3s;
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
  background-image: url(/assets/img/reel-bg.png);
  width: 200px;
  height: 575px;
  clip-path: polygon(0 0, 100% 0, 100% 100%, 0% 100%);
}
.offset {
  transform: translateY(100px);
}
</style>
