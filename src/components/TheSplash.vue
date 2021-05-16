<template>
  <div class="splash">
    <img src="/assets/img/logo-transparent.png" />
    <div class="buttons">
      <h4>Please verify your age to enter.</h4>

      <wide-three-d-button color="red">
        <template v-slot:top>I AM OVER</template>
        <template v-slot:bottom>
          <h3>47</h3>
        </template>
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
</style>