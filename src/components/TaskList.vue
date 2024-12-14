<template>
  <div class="task-app">
    <h1>Task Management App</h1>
    <button @click="showAddTaskForm = !showAddTaskForm">
      {{ showAddTaskForm ? 'Hide Add Task Form' : 'Show Add Task Form' }}
    </button>

    <div v-if="showAddTaskForm" class="add-task-form">
      <input v-model="newTaskName" type="text" placeholder="Enter task name" />
      <button @click="addTask" class="add-task-btn">Add Task</button>
    </div>

    <div v-if="tasks.length === 0" class="no-tasks-message">
      No tasks available
    </div>

    <ul class="task-list">
      <li
        v-for="(task, index) in tasks"
        :key="task.id"
        :class="['task-item', { completed: task.completed }]">
        <div class="task-name">{{ task.name }}</div>
        <div class="task-actions">
          <button 
            v-if="!task.completed" 
            @click="completeTask(index)" 
            class="task-btn complete-btn">
            Complete
          </button>
          <button 
            v-if="task.completed" 
            @click="undoTask(index)" 
            class="task-btn undo-btn">
            Undo
          </button>
          <button 
            @click="deleteTask(index)" 
            class="task-btn delete-btn">
            Delete
          </button>
        </div>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';

const tasks = ref([]);
const newTaskName = ref('');
const showAddTaskForm = ref(false);

onMounted(() => {
  const savedTasks = localStorage.getItem('tasks');
  if (savedTasks) {
    tasks.value = JSON.parse(savedTasks);
  }
});

function addTask() {
  if (newTaskName.value.trim().length >= 3) {
    const task = {
      id: Date.now(),
      name: newTaskName.value,
      completed: false,
    };
    tasks.value.push(task);
    localStorage.setItem('tasks', JSON.stringify(tasks.value));
    newTaskName.value = '';
  } else {
    alert('Task name must be at least 3 characters long.');
  }
}

function completeTask(index) {
  tasks.value[index].completed = true;
  localStorage.setItem('tasks', JSON.stringify(tasks.value));
}

function undoTask(index) {
  tasks.value[index].completed = false;
  localStorage.setItem('tasks', JSON.stringify(tasks.value));
}

function deleteTask(index) {
  tasks.value.splice(index, 1);
  localStorage.setItem('tasks', JSON.stringify(tasks.value));
}
</script>

<style scoped>
* {
  box-sizing: border-box;
}
.task-app {
  background: linear-gradient(to right, #2c3e50, #34495e);
  color: #fff;
  padding: 40px;
  border-radius: 12px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.3);
  text-align: center;
  width: 100%;
  max-width: 1200px;
  margin: 0 auto;
}
h1 {
  font-size: 2.5em;
  margin-bottom: 20px;
}
button {
  background-color: #217346;
  color: #fff;
  padding: 12px 25px;
  border: none;
  border-radius: 6px;
  margin-bottom: 5px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}
button:hover {
  background-color: #1c5c32;
}
.add-task-form {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  gap: 15px;
  margin-bottom: 25px;
  background: linear-gradient(145deg, #3e8e41, #2f6b34);
  padding: 25px;
  border-radius: 10px;
  box-shadow: 0px 4px 12px rgba(0, 0, 0, 0.2);
  width: 100%;
}
.add-task-form input {
  width: 100%;
  padding: 12px;
  border-radius: 6px;
  border: none;
  font-size: 16px;
}
.add-task-form input:focus {
  border: 2px solid #81c784;
}
.add-task-btn {
  width: 100%;
  padding: 12px;
  background-color: #4caf50;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s;
}
.add-task-btn:hover {
  background-color: #388e3c;
}
.task-list {
  list-style-type: none;
  padding: 0;
}
.task-item {
  background-color: #34495e;
  padding: 15px;
  border-radius: 8px;
  margin: 15px 0;
  display: flex;
  justify-content: space-between;
  align-items: center;
  transition: background-color 0.3s, box-shadow 0.3s;
}
.task-item.completed {
  background-color: #2c6d4f;
  color: #d1ffd6;
  box-shadow: 0 0 10px rgba(0, 255, 0, 0.5);
}
.task-name {
  flex: 3;
  text-align: left;
}
.task-actions {
  flex: 1;
  display: flex;
  gap: 10px;
}
.task-btn {
  padding: 10px 20px;
  border-radius: 6px;
  cursor: pointer;
  width: 100px;
}
.complete-btn {
  background-color: #4caf50;
}
.delete-btn {
  background-color: #f44336;
}
.undo-btn {
  background-color: #ffa500;
}
.task-btn:hover {
  opacity: 0.8;
}
.no-tasks-message {
  font-size: 1.2em;
  color: #fff;
}
</style>
