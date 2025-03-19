<template>
  <div class="flex gap-x-4 items-center">
    <progress class="" :value="currentStep" :max="totalSteps"></progress>
    <span>{{ currentStep*100/totalSteps }} %</span>
  </div>
</template>

<script setup>
defineProps({
  currentStep: Number,
  totalSteps: Number,
});
</script>
