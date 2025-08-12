<script setup lang="ts">
import { computed, ref, watch } from 'vue';

const props = defineProps(['question', 'choices', 'correctawn']);
const emits = defineEmits(['answerSelected']);
const answer = ref<string | null>(null);
const hasAnswered = computed(() => answer.value !== null);
watch(() => props.question, () => {
    answer.value = null;
});

const classes = (choice: string) => {
    if (!hasAnswered.value) {
        return {
            'cursor-pointer': true,
        };
    }
    if (choice === props.correctawn) {
        return {
            'bg-green-500 text-white cursor-pointer': true, // Bonne réponse en vert
        };
    }
    else  {
        return {
            'bg-red-500 text-white cursor-pointer': true, // Mauvaise réponse sélectionnée en rouge
        };
    }
};
</script>

<template>
    <div class="p-4 bg-white rounded shadow">
        <h2 class="text-xl font-semibold mb-2">{{ question }}</h2>
        <ul class="list-disc pl-5">
            <li v-for="(choice, index) in choices" :key="choice" class="mb-1">
                <label :for="`answer${index}`" :class="classes(choice)">
                    <input :disabled="hasAnswered" type="radio" :id="`answer${index}`" name="answer" :value="choice" v-model="answer" />
                    {{ choice }}
                </label>
            </li>
        </ul>
        <button :disabled="!hasAnswered"
            class="mt-4 bg-blue-500 text-white px-4 py-2 rounded cursor-pointer disabled:cursor-not-allowed disabled:opacity-50"
            @click="emits('answerSelected', answer)"> Next Question</button>
    </div>

</template>