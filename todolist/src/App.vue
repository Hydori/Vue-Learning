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
      :style="{ textDecoration: item.completed ? 'line-through' : 'none' }"
      v-for="item in todolist"
      v-show="!maskCompletedTasks || !item.completed"
      :key="item.title"
    >
      {{ item.title }}
      {{ item.date }}
      <input
        type="checkbox"
        name=""
        id=""
        v-model="item.completed"
        @change="setComplited(item)"
      />
    </li>
  </ul>
  <input
    type="checkbox"
    name=""
    id=""
    @change="maskCompletedTasks = !maskCompletedTasks"
  />
  Masquer les tâches complétées
</template>

<script setup>
import { ref } from 'vue';

const task = ref();
const allTasksCompleted = ref(false);
const maskCompletedTasks = ref(false);


const todolist = ref([
  {
    title: "Acheter la propriété 'Rue de la Paix'",
    completed: false,
    date: new Date().toISOString().split('T')[0],
  },
  {
    title: "Construire un hôtel sur 'Avenue Foch'",
    completed: true,
    date: new Date().toISOString().split('T')[0],
  },
  {
    title: 'Éviter la case prison',
    completed: false,
    date: new Date().toISOString().split('T')[0],
  },
]);

const sortTaks = () => {
  todolist.value.sort((a, b) =>
    a.completed === b.completed ? 0 : a.completed ? 1 : -1
  );
};

const addTask = () => {
  todolist.value.push({
    title: task.value,
    completed: false,
    date: new Date().toISOString().split('T')[0],
  });
  checkAllTasksCompleted();
  sortTaks();
  task.value = '';
};

const setComplited = () => {
  checkAllTasksCompleted();
  sortTaks();
}
sortTaks();


const checkAllTasksCompleted = () => {
  allTasksCompleted.value = todolist.value.every((item) => item.completed);
};
</script>
