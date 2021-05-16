<template>
  <div class="splash">
    <button @click="playBirdhouse"></button>
  </div>
</template>
<script>
export default {
  data() {},
  inject: ['playBirdhouse'],
};
</script>

<style scoped>
.splash {
  background: center / contain no-repeat url('/assets/img/splash.jpg');
}
</style>