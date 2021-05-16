<template>
  <div>
    <h3>{{ paddedCurrentValue }}</h3>
    credit: {{ credit }}<br />
  </div>
  <!--
  <span
    v-if="n100000Trigger"
    :animation-class="slide"
    @complete="slideComplete(100000)"
    >{{ n100000 }}</span
  ><span
    v-if="n10000Trigger"
    :animation-class="slide"
    @complete="slideComplete(10000)"
    >{{ n10000 }}</span
  ><span
    v-if="n1000Trigger"
    :animation-class="slide"
    @complete="slideComplete(1000)"
    >{{ n1000 }}</span
  ><span
    v-if="n1000Trigger"
    :animation-class="slide"
    @complete="slideComplete(1000)"
    >{{ n1000 }}</span
  ><span
    v-if="n100Trigger"
    :animation-class="slide"
    @complete="slideComplete(100)"
    >{{ n100 }}</span
  ><span
    v-if="n10Trigger"
    :animation-class="slide"
    @complete="slideComplete(10)"
    >{{ n10 }}</span
  ><span
    v-if="n1Trigger"
    :animation-class="slide"
    @complete="slideComplete(10000)"
    >{{ n1 }}</span
  >
  -->
</template>

<script>
//import { mapGetters } from 'vuex';

export default {
  data() {
    return {
      currentValue: 0,
      paddedCurrentValue: 0,
      interval: false,
      /*
      n100000: 0,
      n10000: 0,
      n1000: 0,
      n100: 0,
      n10: 0,
      n1: 0,
      n1Trigger: false,
      interval: false,
      */
    };
  },
  created() {
    this.currentValue = this.credit;
    this.paddedCurrentValue = this.currentValue.toString().padStart(6, '0');
  },
  methods: {
    animateValueChange() {
      clearInterval(this.interval);
      this.interval = window.setInterval(() => {
        if (this.currentValue === this.credit) {
          clearInterval(this.interval);
        } else if (this.currentValue < this.credit) {
          this.currentValue++;
        } else {
          this.currentValue--;
        }

        this.paddedCurrentValue = this.currentValue.toString().padStart(6, '0');
      }, 150);
    },
  },
  watch: {
    credit(newValue) {
      this.animateValueChange(newValue);
    },
  },
  props: ['credit'],
};
</script>

<style scoped>
h3 {
  font-family: 'Overpass Mono', monospace;
  font-size: 48px;
}
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
</style>