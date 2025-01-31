<script setup lang="ts">
import { ref } from 'vue'
import TaskModal from '../components/TaskModal.vue'
import DeleteTaskModal from '../components/DeleteTaskModal.vue'

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

const showTaskModal = ref(false)
const showDeleteModal = ref(false)
const selectedTask = ref<Task | undefined>()

const toggleComplete = (taskId: number) => {
  const task = tasks.value.find((t) => t.id === taskId)
  if (task) {
    task.completed = !task.completed
  }
}

const handleSaveTask = ({ id, title }: { id?: number; title: string }) => {
  if (id) {
    const task = tasks.value.find((t) => t.id === id)
    if (task) {
      task.title = title
    }
  } else {
    const newId = Math.max(0, ...tasks.value.map((t) => t.id)) + 1
    tasks.value.push({
      id: newId,
      title,
      completed: false,
    })
  }
}

const resetSelectedTask = () => {
  selectedTask.value = undefined
  showTaskModal.value = false
  showDeleteModal.value = false
}

const deleteTask = (taskId: number) => {
  const index = tasks.value.findIndex((t) => t.id === taskId)
  if (index !== -1) {
    tasks.value.splice(index, 1)
  }
  resetSelectedTask()
}

const openCreateModal = () => {
  resetSelectedTask()
  showTaskModal.value = true
}

const openEditModal = (task: Task) => {
  selectedTask.value = task
  showTaskModal.value = true
}

const openDeleteModal = (task: Task) => {
  selectedTask.value = task
  showDeleteModal.value = true
}
</script>

<template>
  <div class="tasks-page">
    <h1>Tasks</h1>
    <button class="create-button" @click="openCreateModal">Create New Task</button>
    <div class="tasks-list">
      <div
        v-for="task in tasks"
        :key="task.id"
        class="task-item"
        :class="{ completed: task.completed }"
      >
        <span class="task-title">{{ task.title }}</span>
        <div class="task-actions">
          <button class="icon-button edit" @click="openEditModal(task)" title="Edit">✎</button>
          <button class="icon-button delete" @click="openDeleteModal(task)" title="Delete">
            ×
          </button>
          <button class="complete-button" @click="toggleComplete(task.id)">
            {{ task.completed ? 'Mark Incomplete' : 'Mark Complete' }}
          </button>
        </div>
      </div>
    </div>

    <TaskModal
      :show="showTaskModal"
      :task="selectedTask"
      @close="resetSelectedTask"
      @save="handleSaveTask"
    />

    <DeleteTaskModal
      v-if="selectedTask"
      :show="showDeleteModal"
      :task="selectedTask"
      @close="resetSelectedTask"
      @confirm="deleteTask"
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

.task-actions {
  display: flex;
  gap: 0.5rem;
  align-items: center;
}

.icon-button {
  padding: 0.25rem 0.5rem;
  border: none;
  border-radius: 4px;
  color: white;
  cursor: pointer;
  font-size: 1.2rem;
  line-height: 1;
}

.icon-button.edit {
  background-color: #ffc107;
}

.icon-button.edit:hover {
  background-color: #e0a800;
}

.icon-button.delete {
  background-color: #dc3545;
}

.icon-button.delete:hover {
  background-color: #c82333;
}

.create-button {
  margin: 1rem 0;
  padding: 0.75rem 1.5rem;
  background-color: #2196f3;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 1rem;
  transition: background-color 0.2s;
}

.create-button:hover {
  background-color: #1976d2;
}

.complete-button {
  padding: 0.5rem 1rem;
  border: none;
  border-radius: 4px;
  background-color: #4caf50;
  color: white;
  cursor: pointer;
  transition: background-color 0.2s;
}

.complete-button:hover {
  background-color: #45a049;
}

.completed .complete-button {
  background-color: #666;
}

.completed .complete-button:hover {
  background-color: #555;
}
</style>
