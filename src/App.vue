<script setup lang="ts">
import { ref } from 'vue'
import type { Ref } from 'vue'
import AireCondicionado from './components/AireCondicionado.vue'

const start: Ref<boolean> = ref(false)
</script>

<style lang="scss">
#container {
  height: 100%;

  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;

  background-color: rgb(244, 255, 226);

  .portada {
    h1 {
      font-size: 200px;
    }
  }
}
</style>

<template>
  <div id="container">
    <div class="portada" v-if="start == false">
      <h1>HUELLA*</h1>
      <button class="next" @click="() => (start = true)">Comenzar</button>
    </div>
    <div class="sigueinte" v-else>
      <AireCondicionado></AireCondicionado>
    </div>
  </div>
</template>
