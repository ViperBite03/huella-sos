<script lang="ts" setup>
import { onMounted, defineProps } from 'vue'
import Chart from 'chart.js/auto'

const props = defineProps<{
  calefaccion: number
  aire: number
}>()

const personalData = {
  labels: ['Calefacción', 'Aire acondicionado'],
  datasets: [
    {
      label: 'Consumo personal anual:',
      data: [props.calefaccion, props.aire], // datos que vienen del padre
      backgroundColor: ['#9be8bb', '#fff48f'],
    },
  ],
}

const mediaData = {
  labels: ['Calefacción', 'Aire acondicionado'],
  datasets: [
    {
      label: 'Media española de consumo:',
      data: [5100, 300], // valores medios reales en kWh según IDAE
      backgroundColor: ['#9be8bb', '#fff48f'],
    },
  ],
}

onMounted(() => {
  const personalCtx = (
    document.getElementById('calefaccionChart') as HTMLCanvasElement
  )?.getContext('2d')
  const mediaCtx = (document.getElementById('aireChart') as HTMLCanvasElement)?.getContext('2d')

  if (personalCtx) {
    new Chart(personalCtx, {
      type: 'doughnut',
      data: personalData,
      options: {
        plugins: {
          title: {
            display: true,
            text: `Consumo personal anual: ${props.calefaccion + props.aire} kWh`,
          },
        },
      },
    })
  }

  if (mediaCtx) {
    new Chart(mediaCtx, {
      type: 'doughnut',
      data: mediaData,
      options: {
        plugins: {
          title: {
            display: true,
            text: 'Media española de consumo: 5400 kWh',
          },
        },
      },
    })
  }
})
</script>

<style scoped>
.contenedor {
  height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 25px;

  .charts {
    display: flex;
    flex-wrap: wrap;
    gap: 2rem;
    justify-content: center;
    margin-top: 2rem;

    .chart {
      width: 300px;
      height: 300px;
    }
  }

  .compara {
    font-size: 20px;
    font-weight: bold;
    color: rgb(75, 74, 74);
  }
}
</style>

<template>
  <div class="contenedor">
    <div class="charts">
      <div class="chart">
        <canvas id="calefaccionChart"></canvas>
      </div>
      <div class="chart">
        <canvas id="aireChart"></canvas>
      </div>
    </div>
    <div class="compara">
      {{
        props.calefaccion + props.aire < 200
          ? 'Consumes menos que una máquina de hacer hielo'
          : 'Consumes más que una feria (háztelo mirar)'
      }}
    </div>
  </div>
</template>
