<script setup lang="ts">
import { computed, ref } from 'vue';

defineProps(['question', 'choices']);
const emits = defineEmits(['answerSelected']);
const answer = ref<string | null>(null);
const hasAnswered = computed(() => answer.value !== null);
</script>

<template>
    <div class="p-4 bg-white rounded shadow">
        <h2 class="text-xl font-semibold mb-2">{{ question }}</h2>
        <ul class="list-disc pl-5">
            <li v-for="(choice, index) in choices" :key="choice" class="mb-1">
                <label :for="`answer${index}`" class="cursor-pointer">
                    <input type="radio" :id="`answer${index}`" name="answer" :value="choice" v-model="answer" />
                    {{ choice }}
                </label>
            </li>
        </ul>
        <button :disabled="!hasAnswered"
            class="mt-4 bg-blue-500 text-white px-4 py-2 rounded cursor-pointer disabled:cursor-not-allowed disabled:opacity-50"
            @click="emits('answerSelected', answer)"> Next Question</button>
    </div>

</template>