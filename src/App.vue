<script setup>
import Calculator from './components/Calculator.vue';
</script>

<template>
  <div style="display: flex; flex-direction: column; justify-content: flex-start; height: 100vh">
    <div style="flex-shrink: 1; height:60px;"></div>
    <Calculator />
  </div>
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
