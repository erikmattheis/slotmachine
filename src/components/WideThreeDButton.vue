<template>
  <button>
    <span class="outer">
      <slot></slot>
      <!-- <span class="top"><slot name="top"></slot></span>
      <span class="bottom"><slot name="bottom"></slot></span> -->
    </span>
  </button>
</template>

<script>
export default {
  name: 'WideThreeDButton',
  props: ['color'],
};
</script>

<style scoped>
button {
  color: #482368;
  text-decoration: none;
  font: bold 18px Arial;
  position: relative;
  display: inline-block;
  margin-right: 15px;
  padding: 15px;
  border-radius: 30px;
  width: 185px;
  height: 60px;
  outline: none;
  border-width: 1px;
  transition: all 0.2s ease-in-out;
  background-color: #9663c4;
  box-shadow: 0 8px 0 #884fb9, 0 0 3px rgba(0, 0, 0, 0.2), 0 20px 20px #512379;
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
  box-shadow: none;
  transform: translate3D(0, 8px, 0);
  background: #9663c4;
  box-shadow: none;
}
</style>
