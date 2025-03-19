<template>
  <form action="">
    <fieldset class="flex flex-col gap-y-4">
      <legend class="mb-4">{{ question.question }}</legend>
      <div class="flex flex-col items-start gap-y-2">
        <Radio
          v-for="(choice, i) in question.choices"
          :key="i"
          :choice="choice"
          :qIndex="qIndex"
          :cIndex="i"
          @lockChoice="selectAnswer(choice)"
        />
      </div>
      <!-- <input
        type="submit"
        value="Question suivante"
        class="cursor-pointer ml-auto bg-blue-600 p-2 text-white"
      /> -->
    </fieldset>
  </form>
  <div>
    <Countdown
      :countdownSpeedUp="countdownSpeedUp"
      @countdownOver="$emit('submitAnswer', selectedAnswer)"
      @updateCountdownSpeedUp="countdownSpeedUp = false"
    />
  </div>
</template>

<script setup>
import { ref } from 'vue';
import Countdown from './Countdown.vue';
import Radio from './Radio.vue';

const props = defineProps({
  question: Object,
  qIndex: Number,
});

const countdownSpeedUp = ref(false);

defineEmits(['submitAnswer']);

const selectAnswer = (choice) => {
  selectedAnswer.value = choice;
  countdownSpeedUp.value = true;
};

const selectedAnswer = ref('');
</script>
