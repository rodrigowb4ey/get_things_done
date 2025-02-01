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
  <div v-if="show" 
       class="fixed inset-0 bg-black/50 flex justify-center items-center z-50"
       @click="emit('close')">
    <div class="bg-white p-8 rounded-lg w-[90%] max-w-lg" @click.stop>
      <h2 class="text-2xl font-bold mb-4">Delete Task</h2>
      <p class="mb-6">Are you sure you want to delete the task "{{ task.title }}"?</p>
      <div class="flex justify-end gap-4">
        <button 
          class="px-4 py-2 rounded bg-gray-600 text-white hover:bg-gray-700 transition-colors"
          @click="emit('close')"
        >
          Cancel
        </button>
        <button 
          class="px-4 py-2 rounded bg-red-600 text-white hover:bg-red-700 transition-colors"
          @click="emit('confirm', task.id)"
        >
          Delete
        </button>
      </div>
    </div>
  </div>
</template>