<template>
  <div class="flex items-center justify-center min-h-screen bg-gray-100">
    <div
      v-if="!quizFinished"
      class="flex flex-col gap-y-16 items-start justify-center"
    >
      <h1 class="text-3xl text-red-500 font-bold">{{ title }}</h1>

      <Progress :currentStep="step" :totalSteps="questions.length" />
      <Question
        :question="currentQuestion"
        :key="step"
        :qIndex="step"
        @submitAnswer="(answer) => goToNextQuestion(answer)"
      />
    </div>

    <ResultsPanel
      v-else
      :score="score"
      :scoreMax="questions.length"
      :message="minimum_score <= score ? success_message : failure_message"
    />
  </div>
</template>

<script setup>
import { onMounted, ref } from 'vue';
import Progress from './components/Progress.vue';
import Question from './components/Question.vue';
import ResultsPanel from './components/ResultsPanel.vue';


const failure_message = ref('');
const success_message = ref('');
const questions = ref([]);
const minimum_score = ref(0);
const title = ref('');

const currentQuestion = ref({});
const step = ref(0);
const score = ref(0);
const quizFinished = ref(false);

const goToNextQuestion = (answer) => {
  if (answer === currentQuestion.value.correct_answer) {
    incrementScore();
  }
  incrementStep();
};

const incrementStep = () => {
  if (step.value === questions.value.length - 1) {
    quizFinished.value = true;
  } else {
    step.value += 1;
    currentQuestion.value = questions.value[step.value];
  }
};

const incrementScore = () => {
  score.value += 1;
};

const shuffleChoices = (array) => {
  array.map((item) => {
    console.log(item.choices);
    item.choices.map((choice, index) => {
      const j = Math.floor(Math.random() * (index + 1));
      [item.choices[index], item.choices[j]] = [
        item.choices[j],
        item.choices[index],
      ];
    });
  });
};

onMounted(() => {
  fetch('/quiz.json')
    .then((response) => response.json())
    .then((data) => {
      failure_message.value = data.failure_message;
      success_message.value = data.success_message;
      questions.value = data.questions;
      minimum_score.value = data.minimum_score;
      title.value = data.title;
      currentQuestion.value = questions.value[0];
    })
    .then(() => {
      shuffleChoices(questions.value);
    });
});
</script>
