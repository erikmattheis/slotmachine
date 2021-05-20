<template>
  <div class="splash">
    <img src="/assets/img/logo-transparent.png" />
    <div class="buttons">
      <h4>Please verify your age to enter.</h4>

      <wide-three-d-button color="red">
        <template v-slot
          >I AM OVER <span class="light-purple">47</span></template
        >
      </wide-three-d-button>
    </div>
  </div>
</template>
<script>
import WideThreeDButton from './WideThreeDButton';
export default {
  name: 'TheSplash',
  data() {
    return {};
  },
  components: { WideThreeDButton },
  methods: {},
};
</script>

<style scoped>
img {
  max-width: 100%;
}
.splash {
  width: 100%;
  height: 100vh;
  position: relative;
  text-align: center;
}
.buttons {
  width: 100%;
  position: absolute;
  bottom: 0;
  height: 200px;
  margin: auto;
}

.light-purple {
  color: #d6b1f7;
}
</style>