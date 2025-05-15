<script lang="ts" setup>
import { ref, defineProps } from 'vue'

interface IPregunta {
  question: string
  answers: { title: string; value: number }[]
}

const preguntas: IPregunta[] = [
  {
    question: '¿Qué tipo de aire acondicionado utilizas?',
    answers: [
      { title: 'Aire acondicionado portátil (eléctrico)', value: 0.9 },
      { title: 'Aire acondicionado fijo (split o sistema de pared)', value: 1.2 },
      {
        title:
          'Aire acondicionado centralizado (sistema de climatización central en todo el edificio)',
        value: 2.5,
      },
      {
        title: 'Ventilador (si lo usas para refrescar en lugar de aire acondicionado)',
        value: 0.075,
      },
      { title: 'Otro', value: 0.5 },
      { title: 'No lo sé', value: 0.1 },
    ],
  },
  {
    question: '¿Tu aire acondicionado tiene una buena etiqueta energética?',
    answers: [
      { title: 'Etiqueta energética A o superior (más eficiente)', value: -20 },
      { title: 'Etiqueta energética B o inferior (menos eficiente)', value: 0.1 },
      { title: 'No lo sé / No tiene etiqueta', value: 5 },
      { title: 'No lo sé', value: 0.1 },
    ],
  },
  {
    question: '¿Cuántas horas al día utilizas el aire acondicionado durante los meses cálidos?',
    answers: [
      { title: '1-2 horas al día', value: 1.5 },
      { title: '3-5 horas al día', value: 4 },
      { title: 'Más de 5 horas al día', value: 7 },
    ],
  },
  {
    question: '¿En qué meses del año utilizas el aire acondicionado?',
    answers: [
      { title: 'Solo en verano (junio - septiembre)', value: 3 },
      { title: 'En primavera y otoño también (en días calurosos)', value: 6 },
      { title: 'Todo el año (para mantener temperatura constante en la vivienda)', value: 12 },
    ],
  },
  {
    question: '¿A qué temperatura mantienes el aire acondicionado?',
    answers: [
      { title: 'Menos de 22°C (temperatura más eficiente)', value: -10 },
      { title: 'Entre 22°C y 24°C (temperatura media)', value: 0.1 },
      { title: 'Más de 24°C (temperatura alta, menos eficiente)', value: 5 },
    ],
  },
  {
    question:
      '¿Utilizas algún sistema adicional para refrescar la casa (ventiladores, sistemas de enfriamiento evaporativo, etc.)?',
    answers: [
      { title: 'Sí', value: 0.3 },
      { title: 'No', value: 0.1 },
      { title: 'No lo sé', value: 0.1 },
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
    calcularAC()
  } else {
    scroll.value.style = `transform: translateY(calc(-100vh * ${index + 1}))`
  }
}

const props = defineProps<{
  setValor: (valor: number) => void
}>()

const calcularAC = () => {
  const total = respuestas.value[0] * (respuestas.value[2] * respuestas.value[3])
  const eficiencia = total + (total * respuestas.value[1]) / 100
  const temperatura = total + (total * respuestas.value[4]) / 100

  const final = (eficiencia + temperatura) * 10

  props.setValor(final)

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
