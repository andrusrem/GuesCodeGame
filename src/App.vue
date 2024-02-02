<script setup>
  import { ref, reactive } from 'vue';
  import Quizz from './components/quizz.vue';

  let state = reactive({
    page: 'home',
    points: 0,
  })

  let currentQuestionCount = 0
  let currentQuestion = ref(null)
  let questions = [
    { image: "quizzImages/quizz1.png", variants: ["Loop", "Function", "Array"], answer: "Loop" },
    { image: "quizzImages/quizz2.png", variants: ["Function", "Loop", "Array"], answer: "Array" },
    { image: "quizzImages/guizz3.png", variants: ["Interface", "Class", "Enumirate"], answer: "Class" },
    { image: "quizzImages/quizz4.png", variants: ["Enumirate", "Interface", "Class" ], answer: "Enumirate" },
    { image: "quizzImages/quizz5.png", variants: [ "Class", "Interface", "Enumirate"], answer: "Interface" },
    { image: "quizzImages/quizz6.png", variants: [ "Test Class", "Test Function", "Mingi jama"], answer: "Test Function" },
    { image: "quizzImages/quizz7.png", variants: [ "HTML", "CSS", "Javascript"], answer: "Javascript" },
    { image: "quizzImages/quizz8.png", variants: [ "C++", "C#", "Java"], answer: "C#" },
    { image: "quizzImages/quizz9.png", variants: [ "XML", "HTML", "Juri Trei teab"], answer: "HTML" },
    { image: "quizzImages/quizz10.png", variants: [ "SCSS", "CSS", "Ei tea"], answer: "CSS" },
    

  ]

  // Randomize questions
  questions = questions
    .map(value => ({ value, sort: Math.random() }))
    .sort((a, b) => a.sort - b.sort)
    .map(({ value }) => value)

  function startPlaying() {
    if (currentQuestionCount >= questions.length) {
      return finish()
    }

    state.page = 'quizz'
    currentQuestion.value = questions[currentQuestionCount]
  }

  function restartGame() {
    currentQuestionCount = 0
    state.points = 0
    startPlaying()
  }

  function finish() {
    state.page = 'result'
  }

  function nextPage() {
    currentQuestionCount += 1
    startPlaying()
  }

  function finishMessage() {
    if (state.points == 0) {
      return 'Sorry, oled loll!'
    }
    return 'Palju õnne! Nüüd oled andekas pois/tüdruk!'
  }
</script>

<template>
  <div class="dark:bg-gray-900 dark:text-gray-200 min-h-screen h-full text-4xl flex">
    <div v-if="state.page == 'home'" class="container h-full flex justify-center items-center flex-col max-w-xl m-auto">
      <h1 class="text-3xl font-bold underline mb-20 text-6xl">
        Mis see on?
      </h1>
    <button class="bg-blue-700 hover:bg-blue-800 active:bg-blue-900 max-w-fit py-2 px-8 text-white rounded" @click="startPlaying">Mängi</button>
    </div>
    <Quizz v-if="state.page == 'quizz'" :question="currentQuestion" :state="state" @finish="finish" @nextPage="nextPage" />
    <div v-if="state.page == 'result'" class="flex items-center justify-center h-full flex-col">
      <h1 class="text-3xl font-bold underline mb-20 text-6xl">
        Mis see on?
      </h1>
      <p class="mb-5">{{  finishMessage() }}</p>
      <p class="mb-5">Punktid: {{ state.points }}/{{  questions.length }}</p>
      <button class="bg-blue-700 hover:bg-blue-800 active:bg-blue-900 max-w-fit py-2 px-8 text-white rounded" @click="restartGame">Mängi veel kord</button>
    </div>
  </div>
</template>

<style scoped>

</style>
