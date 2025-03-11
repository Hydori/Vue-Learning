<template>
  <h1>To do list</h1>
  <hr />
  <form action="" @submit.prevent="addTask">
    <input
      type="text"
      placeholder="Ajouter une tâche"
      v-model="task"
      required
    />
    <button>Ajouter</button>
  </form>
  <p v-if="allTasksCompleted">
    On dirait bien que toutes les tâches sont complétées !
  </p>
  <ul>
    <li
      v-for="item in sortTasks"
      :style="{ textDecoration: item.completed ? 'line-through' : 'none' }"
      v-show="!maskCompletedTasks || !item.completed"
      :key="item.date"
    >
      <input
        type="checkbox"
        v-model="item.completed"
        @change="setComplited(item)"
      />
      {{ item.title }}
      {{ item.date }}
    </li>
  </ul>
  <input type="checkbox" name="" id="" v-model="maskCompletedTasks" />
  Masquer les tâches complétées
</template>

<script setup>
import { computed, ref } from 'vue';

const task = ref();
const allTasksCompleted = ref(false);
const maskCompletedTasks = ref(false);

const todolist = ref([
  {
    title: "Acheter la propriété 'Rue de la Paix'",
    completed: false,
    date: 1,
  },
  {
    title: "Construire un hôtel sur 'Avenue Foch'",
    completed: true,
    date: 2,
  },
  {
    title: 'Éviter la case prison',
    completed: false,
    date: 3,
  },
]);

const sortTasks = computed(() => {
  return todolist.value.toSorted((a, b) =>
    a.completed > b.completed ? 1 : -1
  );
});

const addTask = () => {
  todolist.value.push({
    title: task.value,
    completed: false,
    date: new Date(),
  });
  checkAllTasksCompleted();
  task.value = '';
};

const setComplited = () => {
  checkAllTasksCompleted();
};


const checkAllTasksCompleted = () => {
  allTasksCompleted.value = todolist.value.every((item) => item.completed);
};
</script>
