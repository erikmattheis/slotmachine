<template>
  <h3>{{ credit }}</h3>
</template>

<script>
//import { mapGetters } from 'vuex';

export default {
  props: ['credit'],
  computed: {
    // counter() {
    //   return this.$store.getters.finalCounter;
    // },
    //...mapGetters('numbers', ['finalCounter'])
  },
};
</script>