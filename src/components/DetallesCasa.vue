<script lang="ts" setup>
import { ref } from 'vue'

interface IPregunta {
  question: string
  answers: { title: string; value: number }[]
}

const preguntas: IPregunta[] = [
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
