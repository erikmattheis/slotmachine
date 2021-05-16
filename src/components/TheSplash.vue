<template>
  <div class="splash">
    <img src="/assets/img/logo-transparent.png" />
    <div class="buttons">
      <h4>Please verify your age to enter.</h4>
      <button @click="playBirdhouse()">I AM OVER 47</button>
      <button>47</button>
      <button>
        <span class="outer">
          <span class="top">I AM UNDER</span>
          <span class="bottom">47</span>
        </span>
      </button>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {};
  },
  inject: ['playBirdhouse'],
};
</script>

<style scoped>
.splash {
  width: 100%;
  height: 100vh;
}
.buttons {
  text-align: center;
  transform: translateY(-100%);
}
</style>