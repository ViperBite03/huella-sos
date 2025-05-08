<script setup lang="ts">
import paginaUno from './components/paginaUno.vue'

const nextPage = () => {
  console.log('entra')
}
</script>

<style lang="scss">
#grid-container {
  background-image: url('src/assets/leaf-background.jpg');
  height: 100%;

  background-position: center;
  background-size: cover;
  background-repeat: no-repeat;
  padding: 100px;

  .box {
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.65);
    backdrop-filter: blur(8px);
    border-radius: 35px;
    padding: 50px;

    button {
      transition: 0.3s ease;
      position: absolute;
      right: 50px;
      bottom: 50px;

      border: none;
      background-color: violet;
      color: white;
      font-size: 20px;
      padding: 10px 20px;
      border-radius: 8px;
      cursor: pointer;

      &:hover {
        transition: 0.3s ease;
        transform: scale(1.1);
      }

      &:active {
        transition: 0.3s ease;
        transform: scale(1);
      }
    }
  }
}
</style>

<template>
  <div id="grid-container">
    <div class="box">
      <paginaUno></paginaUno>
      <button class="next" @click="nextPage">Continuar</button>
    </div>
  </div>
</template>
