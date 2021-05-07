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
  width: 100px;
  height: 100px;
}
</style>
