<script setup>
    defineProps({
        question: Object,
    })
</script>

<template>
    <div class="flex justify-center items-center flex-col container mx-auto py-20">
        <h1 class="text-6xl mb-8">Mis see on?</h1>
        <img class="w-1/2 mb-8" :src="question.image">
        <div class="flex w-full justify-evenly">
            <div v-for="variant in question.variants">
                {{variant}}
            </div>
        </div>
    </div>
</template>