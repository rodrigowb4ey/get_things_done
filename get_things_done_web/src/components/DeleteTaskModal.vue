<script setup lang="ts">
interface Task {
  id: number
  title: string
}

const props = defineProps<{
  show: boolean
  task: Task
}>()

const emit = defineEmits<{
  close: []
  confirm: [number]
}>()
</script>

<template>
  <div v-if="show" class="modal-backdrop" @click="emit('close')">
    <div class="modal" @click.stop>
      <h2>Delete Task</h2>
      <p>Are you sure you want to delete the task "{{ task.title }}"?</p>
      <div class="button-group">
        <button class="cancel-button" @click="emit('close')">Cancel</button>
        <button class="delete-button" @click="emit('confirm', task.id)">Delete</button>
      </div>
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

.button-group {
  display: flex;
  justify-content: flex-end;
  gap: 1rem;
  margin-top: 1.5rem;
}

button {
  padding: 0.5rem 1rem;
  border: none;
  border-radius: 4px;
  color: white;
  cursor: pointer;
  transition: background-color 0.2s;
}

.cancel-button {
  background-color: #666;
}

.cancel-button:hover {
  background-color: #555;
}

.delete-button {
  background-color: #dc3545;
}

.delete-button:hover {
  background-color: #c82333;
}
</style>
