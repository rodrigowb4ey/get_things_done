<script setup lang="ts">
import { ref } from 'vue'
import CreateTaskModal from '../components/CreateTaskModal.vue'

interface Task {
  id: number
  title: string
  completed: boolean
}

const tasks = ref<Task[]>([
  { id: 1, title: 'Complete project proposal', completed: false },
  { id: 2, title: 'Review pull requests', completed: false },
  { id: 3, title: 'Update documentation', completed: false },
  { id: 4, title: 'Schedule team meeting', completed: false },
  { id: 5, title: 'Prepare weekly report', completed: false },
])

const showCreateModal = ref(false)

const toggleComplete = (taskId: number) => {
  const task = tasks.value.find(t => t.id === taskId)
  if (task) {
    task.completed = !task.completed
  }
}

const createTask = ({ title }: { title: string }) => {
  const newId = Math.max(0, ...tasks.value.map(t => t.id)) + 1
  tasks.value.push({
    id: newId,
    title,
    completed: false
  })
}
</script>

<template>
  <div class="tasks-page">
    <h1>Tasks</h1>
    <button class="create-button" @click="showCreateModal = true">
      Create New Task
    </button>
    <div class="tasks-list">
      <div v-for="task in tasks" 
           :key="task.id" 
           class="task-item"
           :class="{ completed: task.completed }">
        <span class="task-title">{{ task.title }}</span>
        <button @click="toggleComplete(task.id)">
          {{ task.completed ? 'Mark Incomplete' : 'Mark Complete' }}
        </button>
      </div>
    </div>

    <CreateTaskModal
      :show="showCreateModal"
      @close="showCreateModal = false"
      @create="createTask"
    />
  </div>
</template>

<style scoped>
.tasks-page {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 2rem;
}

.create-button {
  margin: 1rem 0;
  padding: 0.75rem 1.5rem;
  background-color: #2196F3;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 1rem;
  transition: background-color 0.2s;
}

.create-button:hover {
  background-color: #1976D2;
}

.tasks-list {
  width: 100%;
  max-width: 600px;
  margin-top: 2rem;
}

.task-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem;
  margin-bottom: 0.5rem;
  border: 1px solid #ddd;
  border-radius: 4px;
  background-color: white;
}

.task-item.completed .task-title {
  text-decoration: line-through;
  color: #888;
}

button {
  padding: 0.5rem 1rem;
  border: none;
  border-radius: 4px;
  background-color: #4CAF50;
  color: white;
  cursor: pointer;
  transition: background-color 0.2s;
}

button:hover {
  background-color: #45a049;
}

.completed button {
  background-color: #666;
}

.completed button:hover {
  background-color: #555;
}
</style>