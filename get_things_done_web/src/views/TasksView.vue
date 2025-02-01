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
  <div class="flex flex-col items-center p-8">
    <h1 class="text-3xl font-bold mb-4">Tasks</h1>
    
    <button 
      class="mb-8 px-6 py-3 bg-blue-600 text-white rounded hover:bg-blue-700 transition-colors text-lg"
      @click="openCreateModal"
    >
      Create New Task
    </button>

    <div class="w-full max-w-2xl">
      <div 
        v-for="task in tasks" 
        :key="task.id" 
        class="mb-3 p-4 bg-white border border-gray-200 rounded-lg shadow-sm flex items-center justify-between"
        :class="{ 'bg-gray-50': task.completed }"
      >
        <span 
          class="text-lg"
          :class="{ 'line-through text-gray-500': task.completed }"
        >
          {{ task.title }}
        </span>
        
        <div class="flex items-center gap-2">
          <button 
            class="p-2 text-lg bg-yellow-500 text-white rounded hover:bg-yellow-600 transition-colors"
            @click="openEditModal(task)" 
            title="Edit"
          >
            ✎
          </button>
          <button 
            class="p-2 text-lg bg-red-500 text-white rounded hover:bg-red-600 transition-colors"
            @click="openDeleteModal(task)" 
            title="Delete"
          >
            ×
          </button>
          <button 
            class="px-4 py-2 rounded transition-colors text-white"
            :class="task.completed ? 
              'bg-gray-600 hover:bg-gray-700' : 
              'bg-green-600 hover:bg-green-700'"
            @click="toggleComplete(task.id)"
          >
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