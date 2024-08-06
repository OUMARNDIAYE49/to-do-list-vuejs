<template>
  <div class="app">
    <h1>To-Do List</h1>
    <form @submit.prevent="addTask">
      <input v-model="newTask.title" placeholder="Nouvelle tâche" required />
      <input v-model="newTask.date" type="date" required />
      <input v-model.number="newTask.progress" type="number" min="0" max="100" placeholder="Avancement (%)" required />
      <select v-model="newTask.priority">
        <option value="low">Faible</option>
        <option value="medium">Moyenne</option>
        <option value="high">Haute</option>
      </select>
      <button type="submit">Ajouter</button>
    </form>
    <ul>
      <li v-for="task in sortedTasks" :key="task.id" class="task">
        <h3>{{ task.title }}</h3>
        <p>Date: {{ task.date }}</p>
        <p>Avancement: {{ task.progress }}%</p>
        <p>Priorité: {{ task.priority }}</p>
        <button @click="removeTask(task.id)">Supprimer</button>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';

const tasks = ref([]);
const newTask = ref({
  title: '',
  date: '',
  progress: 0,
  priority: 'low',
});

const addTask = () => {
  if (newTask.value.title && newTask.value.date && newTask.value.progress !== null) {
    tasks.value.push({
      ...newTask.value,
      id: Date.now(), // unique ID
    });
    newTask.value = {
      title: '',
      date: '',
      progress: 0,
      priority: 'low',
    };
  }
};

const removeTask = (id) => {
  tasks.value = tasks.value.filter(task => task.id !== id);
};

// Compute sorted tasks based on priority
const sortedTasks = computed(() => {
  return [...tasks.value].sort((a, b) => {
    const priorityOrder = { low: 1, medium: 2, high: 3 };
    return priorityOrder[b.priority] - priorityOrder[a.priority];
  });
});
</script>

<style>
.app {
  max-width: 600px;
  margin: 0 auto;
  padding: 1rem;
}

form {
  display: flex;
  flex-direction: column;
}

input, select, button {
  margin-bottom: 0.5rem;
}

.task {
  border: 1px solid #ddd;
  padding: 1rem;
  margin-bottom: 1rem;
}

button {
  background-color: #f44336;
  color: white;
  border: none;
  padding: 0.5rem;
  cursor: pointer;
}
</style>
