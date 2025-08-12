<script setup lang="ts">
import { computed, ref } from 'vue';
import ProgressionBar from './progression-bar.vue';
import Questions from './questions.vue';
import Recap from './recap.vue';

const props = defineProps<{ quiz: any }>();

const stape = ref<number>(0);
const answers = ref<string[]>(props.quiz.questions.map(() => null))

const question = computed(() => {
    return props.quiz.questions[stape.value]
});
const addAnswer = (answer: string) => {
    answers.value[stape.value] = answer;
    stape.value++;


};

</script>

<template>
    <div>
        <h1 class="text-2xl font-bold"> {{ quiz?.title }}</h1>
        <ProgressionBar class="" :progression="stape && quiz.questions ? ((stape / quiz.questions.length) * 100) : 0" />
        <Questions v-if="stape !== quiz.questions.length" :question="question.question" :choices="question.choices" :correctawn="question.correct_answer"
            @answerSelected="addAnswer" />
        <Recap v-else :answers="answers" :quiz="quiz" />


    </div>
</template>