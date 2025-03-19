<template>
  <div class="flex items-center gap-x-2">
    <input
      type="radio"
      :name="`question-${qIndex}`"
      :id="`choice-${qIndex}-${cIndex}`"
      @change="() => $emit('lockChoice')"
      required
    />
    <label :for="`choice-${qIndex}-${cIndex}`">{{ choice }}</label>
  </div>
</template>

<script setup>
defineProps({
  choice: String,
  qIndex: Number,
  cIndex: Number,
});

defineEmits(['lockChoice']);
</script>
