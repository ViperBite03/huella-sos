<script setup lang="ts">
import { ref } from 'vue'
import type { Ref } from 'vue'
import DetallesCasa from './components/DetallesCasa.vue'
import Calefaccion from './components/CalefaccionC.vue'
import AireAcondicionado from './components/AireAcondicionado.vue'
import Chart from './components/ChartComponent.vue'

const page: Ref<number> = ref(0)
const calefaccion = ref(0)
const AC = ref(0)

const setCalefaccion = (valor: number) => {
  calefaccion.value = valor
}

const setAC = (valor: number) => {
  AC.value = valor
}
</script>

<style lang="scss">
#container {
  position: relative;
  height: 100vh;
  overflow: hidden;

  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;

  background: linear-gradient(
    75deg,
    rgba(185, 217, 235, 0.5) 0%,
    rgba(155, 232, 187, 0.5) 50%,
    rgba(255, 244, 143, 0.5) 100%
  );

  background-color: white;

  .portada {
    h1 {
      font-size: 200px;
      font-weight: bolder;
    }
  }

  .siguiente {
    h2 {
      font-weight: bolder;
      font-size: 25px;
      position: absolute;
      top: 25px;
      left: 50px;
    }

    height: fit-content;
  }
}
</style>

<template>
  <div id="container">
    <div class="portada" v-if="page === 0">
      <h1>HUELLA*</h1>
      <button class="next" @click="() => (page = 1)">Comenzar</button>
    </div>

    <div class="siguiente" v-else-if="page === 1">
      <h2>HUELLA*</h2>
      <DetallesCasa @nextPage="() => page++"></DetallesCasa>
    </div>

    <div class="siguiente" v-else-if="page === 2">
      <h2>HUELLA*</h2>
      <Calefaccion @nextPage="() => page++" :setValor="setCalefaccion" />
    </div>

    <div class="siguiente" v-else-if="page === 3">
      <h2>HUELLA*</h2>
      <AireAcondicionado @nextPage="() => page++" :setValor="setAC" />
    </div>

    <div class="siguiente" v-else>
      <h2>HUELLA*</h2>
      <Chart :calefaccion="calefaccion" :aire="AC" />
    </div>
  </div>
</template>
