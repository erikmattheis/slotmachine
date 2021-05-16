<template>
  <button>
    <span class="outer">
      <span class="top">PLAY </span>
      <span class="bottom">ONE CREDIT</span>
    </span>
  </button>
</template>

<script>
export default {
  props: ['color'],
};
</script>

<style scoped>
button {
  color: black;
  text-decoration: none;
  font: bold 18px Arial;
  position: relative;
  display: inline-block;
  margin-right: 15px;
  padding: 15px;
  border-radius: 85px;
  width: 85px;
  height: 85px;
  outline: none;
  transition: all 0.2s ease-in-out;
  background: #a6e9f7;
  box-shadow: 0 8px 0 #529dad, 0 0 3px rgba(0, 0, 0, 0.2), 0 20px 20px #eee;
}
button span.outer {
  text-align: center;
  width: 100%;
  display: block;
  position: relative;
  top: 50%;
  transform: translateY(-50%);
}
button span.outer span.top {
  display: block;
  padding-bottom: 4px;
}
button span.outer span.bottom {
  border-top: 1px solid black;
  padding-top: 4px;
  display: block;
  text-transform: uppercase;
  line-height: 12px;
  font-size: 60%;
}
button:hover {
  background: #9cc62b;
  box-shadow: none;
  transform: translate3D(0, 8px, 0);
  background: #66cbe1;
  box-shadow: none;
}
</style>
