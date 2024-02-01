<script setup>
    import { ref } from 'vue';

    const emit = defineEmits('finish', 'nextPage')
    let {state, question} = defineProps({
        question: Object,
        state: Object,
    })

    let currentAnswer = ref(null)

    let interval
    let time = ref(30)
    function startTimer() {
        clearInterval(interval)
        interval = setInterval(() => {
            time.value--
            if(time.value == 0) {
                clearInterval(interval)
                result(question, null)
            }
        }, 1000)
    }

    function reset() {
        currentAnswer.value = null
        time.value = 30
        startTimer()
    }

    reset()

    function startDrag(event, item) {
        event.dataTransfer.dropEffect = 'move'
        event.dataTransfer.effectAllowed = 'move'
        event.dataTransfer.setData('answer', item)
    }

    function onDrop(event, question) {
        let answer = event.dataTransfer.getData('answer')
        currentAnswer.value = answer
        result(question, answer)
    }
    function result(question, answer) {
        if(answer == question.answer) {
            state.points += 1
        }

        setTimeout(() => {
            reset()
            emit('nextPage')
        }, 1000);
    }
</script>

<template>
    <div class="flex justify-center items-center flex-col container mx-auto py-20">
        <h1 class="text-6xl mb-8">Mis see on?</h1>
        <img class="w-1/2 min-w-[640px] mb-8" :src="question.image">
        <div 
            class="flex w-full items-center justify-center mb-20" @drop="onDrop($event, question)"
            @dragover.prevent
            @dragenter.prevent
        >
            <p class="mr-10">Aeg : {{time}}</p>
            <p v-if="!currentAnswer" class="text-gray-300">Answer</p>
            <p v-else :class="currentAnswer == question.answer ? 'text-green-600' : 'text-red-600'" >{{currentAnswer}}</p>
            <p class="ml-10">Punktid : {{state.points}}</p>
        </div>
        <div class="flex w-full justify-evenly">
            <div v-for="variant in question.variants" draggable="true" @dragstart="startDrag($event, variant)">
                {{variant}}
            </div>
        </div>
    </div>
</template>