<script setup>
  import { ref } from 'vue';
  import Quizz from './components/quizz.vue';

  let playing = ref(false)
  let currentQuestionCount = 0
  let currentQuestion = ref(null)
  let questions = [
    { image: "quizzImages/quizz1.png", variants: ["Loop", "Function", "Array"], answer: "Loop" }
  ]

  // Randomize questions
  questions = questions
    .map(value => ({ value, sort: Math.random() }))
    .sort((a, b) => a.sort - b.sort)
    .map(({ value }) => value)

  function startPlaying() {
    playing.value = true
    currentQuestion.value = questions[currentQuestionCount]
  }
</script>

<template>
  <div class="dark:bg-gray-900 dark:text-gray-200 h-screen text-4xl">
    <div v-if="!playing" class="container h-full flex justify-center items-center flex-col max-w-xl m-auto">
      <h1 class="text-3xl font-bold underline mb-20 text-6xl">
        Mis see on?
      </h1>
    <button class="bg-blue-700 hover:bg-blue-800 active:bg-blue-900 max-w-fit py-2 px-8 text-white rounded" @click="startPlaying">Mängi</button>
    </div>
    <Quizz v-if="playing" :question="currentQuestion" />
  </div>
</template>

<style scoped>

</style>
