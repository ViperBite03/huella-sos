<script lang="ts" setup>
import { ref } from 'vue'
import type { Ref } from 'vue'

interface IPregunta {
  title: string
  answers: string[]
  correct: number
}

const preguntas: IPregunta[] = [
  { title: 'a', answers: ['', '', ''], correct: 2 },
  { title: 'd', answers: ['', '', ''], correct: 2 },
  { title: 's', answers: ['', '', ''], correct: 2 },
  { title: 'b', answers: ['', '', ''], correct: 2 },
]

const page: Ref<number> = ref(0)

const nextPage = () => {
  page.value += 1
  console.log(page.value)
}
</script>

<style scoped lang="scss"></style>

<template>
  <div class="content-container" v-for="pregunta in preguntas" :key="pregunta.title">
    <span class="q-title"> {{ pregunta.title }} </span>
    <span class="q-answers" v-for="answer in pregunta.answers" :key="answer">
      {{ answer }}
    </span>
    <button class="next" onclick="nextPage()">Next</button>
  </div>
</template>
