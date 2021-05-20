<template>
  <li><img :src="url" /></li>
</template>

<script>
export default {
  props: ['url'],
};
</script>

<style scoped>
img {
  width: 200px;
  height: 275px;
}
</style>
