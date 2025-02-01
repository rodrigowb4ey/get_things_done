<script setup lang="ts">
import { ref, watch } from 'vue'

interface Task {
  id: number
  title: string
  completed: boolean
}

const props = defineProps<{
  show: boolean
  task?: Task
}>()

const emit = defineEmits<{
  close: []
  save: [{ id?: number; title: string }]
}>()

const taskTitle = ref('')

// Update form when task prop changes
watch(
  () => props.show,
  (newValue) => {
    if (newValue && props.task) {
      taskTitle.value = props.task.title
    } else {
      taskTitle.value = ''
    }
  },
)

const handleSubmit = () => {
  if (taskTitle.value.trim()) {
    emit('save', {
      id: props.task?.id,
      title: taskTitle.value.trim(),
    })
    taskTitle.value = ''
    emit('close')
  }
}

const handleClose = () => {
  taskTitle.value = ''
  emit('close')
}
</script>

<template>
  <div v-if="show" 
       class="fixed inset-0 bg-black/50 flex justify-center items-center z-50"
       @click="handleClose">
    <div class="bg-white p-8 rounded-lg w-[90%] max-w-lg" @click.stop>
      <h2 class="text-2xl font-bold mb-4">{{ task ? 'Edit Task' : 'Create New Task' }}</h2>
      <form @submit.prevent="handleSubmit">
        <div class="mb-4">
          <label for="taskTitle" class="block mb-2">Task Title:</label>
          <input
            id="taskTitle"
            v-model="taskTitle"
            type="text"
            placeholder="Enter task title"
            required
            class="w-full p-2 border border-gray-300 rounded focus:outline-none focus:ring-2 focus:ring-blue-500"
          />
        </div>
        <div class="flex justify-end gap-4 mt-6">
          <button 
            type="button" 
            class="px-4 py-2 rounded bg-gray-600 text-white hover:bg-gray-700 transition-colors"
            @click="handleClose"
          >
            Cancel
          </button>
          <button 
            type="submit"
            class="px-4 py-2 rounded bg-green-600 text-white hover:bg-green-700 transition-colors"
          >
            {{ task ? 'Save' : 'Create Task' }}
          </button>
        </div>
      </form>
    </div>
  </div>
</template>