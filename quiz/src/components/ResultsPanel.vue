<template>
  <div>
    <h2>Bravo</h2>

    <p>Votre score est de {{ score }}/{{ scoreMax }}</p>
    <p>{{ message }}</p>
  </div>

</template>
<script setup>
defineProps({
  score: Number,
  scoreMax: Number,
  message: String,
});

</script>
