<script setup>
import sk from './components/skin.vue'
// import up from './components/up.vue'
</script>

<template>
  <div id="top">

    <img src="../public/slemons.png" class="logo">
    <h2>皮肤包生成器</h2>
  </div>
  <div id="box1"></div>
  <!-- <up></up> -->
  <sk></sk>
</template>


<style >
#top {
  position: absolute;
  top: 0;
  left: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100%;
  background-color: #171615;
  z-index: 20;
}

#box1 {
  height: 50px;
  width: 100%;
  background-color: #17161500;
}

.logo {
  height: 3em;
  padding: 1em;
  will-change: filter;
  transition: filter 300ms;
}

h2 {
  color: #f7c15d;
  text-align: center;
  margin: 0;
  padding: 0;
}

.logo:hover {
  filter: drop-shadow(0 0 2em #f7c15d);

}

body {
  background-image: url('../src/assets/bg.png');
  background-repeat: no-repeat;
  background-size: cover;
  background-position: center;
}</style>
