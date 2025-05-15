<script lang="ts" setup>
import { ref } from 'vue'

interface IPregunta {
  question: string
  answers: { title: string; value: number }[]
}

const preguntas: IPregunta[] = [
  {
    question: '¿Qué tipo de aire acondicionado utilizas?',
    answers: [
      { title: 'Aire acondicionado portátil (eléctrico)', value: 0.9 }, // ⚡ Potència estimada (kWh/h)
      { title: 'Aire acondicionado fijo (split o sistema de pared)', value: 1.2 }, // ⚡ Potència estimada
      {
        title:
          'Aire acondicionado centralizado (sistema de climatización central en todo el edificio)',
        value: 2.5,
      }, // ⚡ Potència estimada
      {
        title: 'Ventilador (si lo usas para refrescar en lugar de aire acondicionado)',
        value: 0.075,
      }, // ⚡ Potència estimada
      { title: 'Otro', value: 0.5 }, // ⚡ Estimació genèrica
    ],
  },
  {
    question: '¿Tu aire acondicionado tiene una buena etiqueta energética?',
    answers: [
      { title: 'Etiqueta energética A o superior (más eficiente)', value: -20 }, // 📊 Ajust percentual (-20%)
      { title: 'Etiqueta energética B o inferior (menos eficiente)', value: 0 }, // 📊 Sense ajust
      { title: 'No lo sé / No tiene etiqueta', value: 5 }, // 📊 Ajust percentual (+5%)
    ],
  },
  {
    question: '¿Cuántas horas al día utilizas el aire acondicionado durante los meses cálidos?',
    answers: [
      { title: '1-2 horas al día', value: 1.5 }, // 📊 Mitjana d'hores (per multiplicar el consum/hora)
      { title: '3-5 horas al día', value: 4 }, // 📊 Mitjana d'hores
      { title: 'Más de 5 horas al día', value: 7 }, // 📊 Mitjana d'hores
    ],
  },
  {
    question: '¿En qué meses del año utilizas el aire acondicionado?',
    answers: [
      { title: 'Solo en verano (junio - septiembre)', value: 1 }, // 📊 Multiplicador base
      { title: 'En primavera y otoño también (en días calurosos)', value: 1.5 }, // 📊 Més mesos → més consum
      { title: 'Todo el año (para mantener temperatura constante en la vivienda)', value: 2 }, // 📊 Tot l’any → el doble
    ],
  },
  {
    question: '¿A qué temperatura mantienes el aire acondicionado?',
    answers: [
      { title: 'Menos de 22°C (temperatura más eficiente)', value: -10 }, // 📊 Ajust percentual (-10%)
      { title: 'Entre 22°C y 24°C (temperatura media)', value: 0 }, // 📊 Neutre
      { title: 'Más de 24°C (temperatura alta, menos eficiente)', value: 5 }, // 📊 Ajust percentual (+5%)
    ],
  },
  {
    question: '¿Qué tamaño tiene el área que enfrías con el aire acondicionado?',
    answers: [
      { title: 'Menos de 20 m²', value: 0.5 }, // 📊 Multiplicador segons mida
      { title: '20 - 50 m²', value: 1 },
      { title: '50 - 100 m²', value: 2 },
      { title: 'Más de 100 m²', value: 3 },
    ],
  },
  {
    question:
      '¿Utilizas algún sistema adicional para refrescar la casa (ventiladores, sistemas de enfriamiento evaporativo, etc.)?',
    answers: [
      { title: 'Sí', value: 0.3 }, // ⚡ Potència extra aproximada (kWh/h)
      { title: 'No', value: 0 },
    ],
  },
]

const scroll = ref(null)
const emit = defineEmits(['nextPage'])
const respuestas = ref([])

const pushRespuestas = (valor) => {
  respuestas.value.push(valor)

  console.log(respuestas.value)
}

const nextPage = (index) => {
  if (index == preguntas.length - 1) {
    emit('nextPage')
    calcularCalefaccion()
  } else {
    scroll.value.style = `transform: translateY(calc(-100vh * ${index + 1}))`
  }
}

const calcularCalefaccion = () => {
  const total = respuestas.value[0] * (respuestas.value[2] * respuestas.value[3])
  const eficiencia = total - (total * respuestas.value[1]) / 100
  const temperatura = total + (total * respuestas.value[4]) / 100

  const final = eficiencia + temperatura

  return final
}
</script>

<style scoped lang="scss">
.content-container {
  position: absolute;
  transition: 0.75s ease;
  width: 100%;

  display: flex;
  flex-direction: column;

  height: fit-content;
  padding-top: 50px;

  top: -50px;
  left: 750px;

  .pregunta {
    height: 100vh;
    width: 500px;

    display: flex;
    flex-direction: column;
    justify-content: center;
    gap: 15px;

    .q-question {
      font-size: 20px;
      font-weight: bold;
    }

    .q-answers {
      display: flex;
      gap: 10px;
    }
  }
}
</style>

<template>
  <div class="content-container" ref="scroll">
    <div class="pregunta" v-for="(pregunta, pindex) in preguntas" :key="pindex">
      <span class="q-question">* {{ pregunta.question }} </span>
      <div class="answer-list">
        <div class="q-answers" v-for="(answer, aindex) in pregunta.answers" :key="aindex">
          <input
            type="radio"
            :id="aindex"
            :name="`respuesta-${pindex}`"
            @click="() => pushRespuestas(answer.value)"
          />
          <label :for="index" style="margin-left: 10px">{{ answer.title }}</label>
        </div>
      </div>

      <button class="next" @click="nextPage(pindex)">Next</button>
    </div>
  </div>
</template>
