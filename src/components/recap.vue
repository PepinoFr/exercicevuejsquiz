<script setup lang="ts">
import type { Quiz } from '../App.vue';

//const props = defineProps(['answers', quiz : Quiz]);
const props = defineProps<{
    answers: string[];
    quiz: Quiz;
}>();
const score = props.quiz.questions.reduce((acc, question, index) => {
    return acc + (props.answers[index] === question.correct_answer ? 1 : 0);
}, 0);
</script>
<template>
    <div>
        <h2 class="text-2xl font-bold mb-4">Recap</h2>
        <ul class="list-disc pl-5">
            <li v-for="(answer, index) in answers" :key="index" class="mb-2">
                <strong>Question {{ index + 1 }}:</strong> {{ quiz.questions[index].question }}<br />
                <strong>Your Answer:</strong> {{ answer }}
            </li>
        </ul>
        <p class="mt-4">
            Your score: {{ score }} / {{ quiz.questions.length }}
        </p>
        <p v-if="score >= quiz.minimum_score" class="text-green-600">
            {{ quiz.success_message }}
        </p>
        <p v-else class="text-red-600">
            {{ quiz.failure_message }}
        </p>
    </div>
</template>