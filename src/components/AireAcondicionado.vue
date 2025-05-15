<script lang="ts" setup>
import { ref } from 'vue'

interface IPregunta {
  question: string
  answers: { title: string; value: number }[]
}

const preguntas: IPregunta[] = [
  {
    question: '¿Qué tipo de calefacción utilizas en casa?',
    answers: [
      { title: 'Calefacción eléctrica (preguntas, estufas eléctricas, etc.)', value: 1.5 }, // kWh/h aprox.
      { title: 'Calefacción a gas (gas natural, butano, propano, etc.)', value: 10 }, // kWh/h
      { title: 'Calefacción de biomasa (pellets, madera, etc.)', value: 8 }, // kWh/h aprox.
      { title: 'Calefacción por suelo radiante (eléctrico o a gas)', value: 7 }, // kWh/h (aprox. media entre ambas)
      { title: 'Calefacción centralizada (sistema comunitario)', value: 10 }, // kWh/h por vivienda
      { title: 'Otro', value: 0.35 }, // kWh/h estimado
    ],
  },
  {
    question: '¿Tu sistema de calefacción es eficiente?',
    answers: [
      { title: '¿Es un sistema de calefacción con etiqueta energética A o superior?', value: -20 }, // % reducción de consumo
      {
        title:
          '¿Es un sistema de calefacción de baja temperatura o tecnología eficiente (por ejemplo, bombas de calor, calderas de condensación, etc.)?',
        value: -40,
      }, // % reducción aprox.
    ],
  },
  {
    question: '¿Cuántas horas al día utilizas la calefacción durante los meses fríos?',
    answers: [
      { title: '1-2 horas al día', value: 5 }, // kWh/día
      { title: '3-5 horas al día', value: 10 }, // kWh/día aprox.
      { title: 'Más de 5 horas al día', value: 22 }, // kWh/día aprox.
    ],
  },
  {
    question: '¿En qué meses del año utilizas la calefacción?',
    answers: [
      { title: 'Octubre - Marzo', value: 12 }, // kWh/día media aprox.
      { title: 'Solo en invierno (enero - febrero)', value: 9 }, // kWh/día estimado medio
      {
        title: 'Todo el año (si usas calefacción para regular temperatura durante todo el año)',
        value: 15,
      }, // kWh/día alto (estimado)
    ],
  },
  {
    question: '¿A qué temperatura mantienes la calefacción?',
    answers: [
      { title: '18-20°C (baja temperatura)', value: 0 }, // consumo base
      { title: '21-23°C (temperatura media)', value: 5 }, // % incremento estimado
      { title: 'Más de 23°C (alta temperatura)', value: 10 }, // % incremento estimado
    ],
  },
  {
    question: '¿Tu vivienda tiene buen aislamiento térmico?',
    answers: [
      { title: 'Sí, tengo aislamiento en paredes y ventanas', value: -15 }, // % reducción de consumo
      { title: 'No, mi vivienda no está bien aislada', value: 0 }, // sin reducción
    ],
  },
  {
    question: '¿Tienes ventanas de doble cristal o aislamiento adicional en ventanas?',
    answers: [
      { title: 'Sí, tengo ventanas de doble cristal', value: -10 }, // % reducción de consumo
      { title: 'No, tengo ventanas de un solo cristal', value: 0 }, // sin reducción
    ],
  },
  {
    question:
      '¿Tienes persianas, cortinas o estores que ayuden a mantener el calor dentro de la vivienda?',
    answers: [
      { title: 'Sí', value: -5 }, // % reducción de consumo
      { title: 'No', value: 0 },
    ],
  },
  {
    question: 'b',
    answers: [
      { title: '', value: 0 },
      { title: '', value: 0 },
      { title: '', value: 0 },
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
