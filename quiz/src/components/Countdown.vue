<template>
  <p>Compteur : {{ count }}</p>
</template>
<script setup>
import { onMounted, onUnmounted, ref, watch } from 'vue';

const props = defineProps({
  countdownSpeedUp: Boolean,
});

const emit = defineEmits(['countdownOver', 'updateCountdownSpeedUp']);

watch(() => props.countdownSpeedUp, (newVal) => {
  if (newVal) {
    setLoopTo1();
    emit('updateCountdownSpeedUp');
  }
});

const count = ref(3);
let loop = null;

const startLoop = () => {
  loop = setInterval(() => {
    if (count.value === 0) {
      stopLoop();
      emit('countdownOver');
    } else {
      count.value -= 1;
    }
  }, 1000);
};

const stopLoop = () => {
  clearInterval(loop);
};

const setLoopTo1 = () => {
  count.value = 1;
  stopLoop();
  startLoop();
};

onMounted(startLoop);

onUnmounted(stopLoop);
</script>
