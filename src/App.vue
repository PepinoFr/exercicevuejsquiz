<script setup lang="ts">
import { onMounted, ref } from 'vue';
import Quiz from './components/quiz.vue';
interface Question {
  question: string;
  choices: string[];
  correct_answer: string;
}

export interface Quiz {
  title: string;
  minimum_score: number;
  success_message: string;
  failure_message: string;
  questions: Question[];
}
const quiz = ref<Quiz | null>(null);
const state = ref<string>("loading");
onMounted(() => {
  fetch("http://localhost:5173/public/quiz.json").then((response) => {
    if (!response.ok) {
      throw new Error("Network response was not ok")
    }
    return response.json();
  }).then(async (response) => {


    await new Promise((resolve) => {
      setTimeout(() => {
        resolve(true);
      }, 1000);
    });
    quiz.value = response
    state.value = "idle";
  }).catch((error) => {
    console.error("There was a problem with the fetch operation:", error);
    state.value = "error";
  });
});

</script>

<template>
  <div class="min-h-screen bg-gray-100 flex items-center justify-center p-4">
    <div v-if="state === 'error'">
      <p>
        Importing quiz data from JSON file.
      </p>
    </div>
    <div v-if="state === 'idle'" class="">
      <Quiz :quiz="quiz" v-if="quiz" />
    </div>
    <div v-else-if="state === 'loading'">
      <div type="button"
        class="inline-flex  items-center rounded-md bg-indigo-500 px-4 py-2 text-sm leading-6 font-semibold text-white transition duration-150 ease-in-out hover:bg-indigo-400"
        ><svg class="mr-3 -ml-1 size-5 animate-spin text-white" xmlns="http://www.w3.org/2000/svg"
          fill="none" viewBox="0 0 24 24">
          <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
          <path class="opacity-75" fill="currentColor"
            d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z">
          </path>
        </svg>Processing…</div>

    </div>
  </div>
</template>
