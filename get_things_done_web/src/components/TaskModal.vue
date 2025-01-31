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
  <div v-if="show" class="modal-backdrop" @click="handleClose">
    <div class="modal" @click.stop>
      <h2>{{ task ? 'Edit Task' : 'Create New Task' }}</h2>
      <form @submit.prevent="handleSubmit">
        <div class="form-group">
          <label for="taskTitle">Task Title:</label>
          <input
            id="taskTitle"
            v-model="taskTitle"
            type="text"
            placeholder="Enter task title"
            required
          />
        </div>
        <div class="button-group">
          <button type="button" class="cancel-button" @click="handleClose">Cancel</button>
          <button type="submit" class="submit-button">
            {{ task ? 'Save' : 'Create Task' }}
          </button>
        </div>
      </form>
    </div>
  </div>
</template>

<style scoped>
.modal-backdrop {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 100;
}

.modal {
  background-color: white;
  padding: 2rem;
  border-radius: 8px;
  width: 90%;
  max-width: 500px;
}

.form-group {
  margin-bottom: 1rem;
}

label {
  display: block;
  margin-bottom: 0.5rem;
}

input {
  width: 100%;
  padding: 0.5rem;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 1rem;
}

.button-group {
  display: flex;
  justify-content: flex-end;
  gap: 1rem;
  margin-top: 1.5rem;
}

.cancel-button {
  background-color: #666;
}

.cancel-button:hover {
  background-color: #555;
}

.submit-button {
  background-color: #4caf50;
}

.submit-button:hover {
  background-color: #45a049;
}

button {
  padding: 0.5rem 1rem;
  border: none;
  border-radius: 4px;
  color: white;
  cursor: pointer;
  transition: background-color 0.2s;
}
</style>
