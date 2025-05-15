<script lang="ts" setup>
import { onMounted } from 'vue'
import Chart from 'chart.js/auto'

const calefaccionData = {
  labels: ['Eléctrica', 'Gas natural', 'Biomasa', 'Suelo radiante', 'Centralizada', 'Otro'],
  datasets: [
    {
      label: 'Consumo Calefacción',
      data: [600, 1125, 800, 900, 1000, 300], // valores estimados de consumo anual en kWh
      backgroundColor: ['#FF6384', '#36A2EB', '#FFCE56', '#4BC0C0', '#9966FF', '#FF9F40'],
    },
  ],
}

const aireData = {
  labels: ['Portátil', 'Fijo (split)', 'Centralizado', 'Ventilador', 'Otro'],
  datasets: [
    {
      label: 'Consumo Aire Acondicionado',
      data: [600, 480, 1000, 100, 200], // valores estimados de consumo anual en kWh
      backgroundColor: ['#FF9F40', '#4BC0C0', '#9966FF', '#C9CBCF', '#FF6384'],
    },
  ],
}

onMounted(() => {
  const calefCtx = (document.getElementById('calefaccionChart') as HTMLCanvasElement)?.getContext(
    '2d',
  )
  const aireCtx = (document.getElementById('aireChart') as HTMLCanvasElement)?.getContext('2d')

  if (calefCtx) {
    new Chart(calefCtx, {
      type: 'doughnut',
      data: calefaccionData,
      options: {
        plugins: {
          title: {
            display: true,
            text: 'Consumo de Calefacción por Tipo',
          },
        },
      },
    })
  }

  if (aireCtx) {
    new Chart(aireCtx, {
      type: 'doughnut',
      data: aireData,
      options: {
        plugins: {
          title: {
            display: true,
            text: 'Consumo de Aire Acondicionado por Tipo',
          },
        },
      },
    })
  }
})
</script>

<template>
  <div class="charts">
    <div class="chart">
      <canvas id="calefaccionChart"></canvas>
    </div>
    <div class="chart">
      <canvas id="aireChart"></canvas>
    </div>
  </div>
</template>

<style scoped>
.charts {
  display: flex;
  flex-wrap: wrap;
  gap: 2rem;
  justify-content: center;
  margin-top: 2rem;
}

.chart {
  width: 300px;
  height: 300px;
}
</style>
