<template>
  <div class="flex items-center gap-4 p-4 hover:bg-gray-50 transition-colors">
    <!-- Status Icon -->
    <div
      @click="$emit('toggle', todo.id)"
      class="w-10 h-10 flex items-center justify-center rounded cursor-pointer bg-gray-200 hover:bg-gray-300 transition-colors"
    >
      <span class="text-xl">{{ statusIcon }}</span>
    </div>

    <!-- Title -->
    <div class="flex-1">
      <p class="text-gray-900">{{ todo.title }}</p>
    </div>

    <!-- Action Buttons -->
    <div class="flex gap-2">
      <!-- Edit Button -->
      <button
        @click="$emit('edit', todo.id)"
        class="flex items-center gap-1 px-3 py-1.5 text-sm text-blue-600 hover:bg-blue-50 rounded transition-colors"
      >
        <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M11 5H6a2 2 0 00-2 2v11a2 2 0 002 2h11a2 2 0 002-2v-5m-1.414-9.414a2 2 0 112.828 2.828L11.828 15H9v-2.828l8.586-8.586z" />
        </svg>
        Edit
      </button>

      <!-- Delete Button -->
      <button
        @click="$emit('delete', todo.id)"
        class="flex items-center gap-1 px-3 py-1.5 text-sm text-red-600 hover:bg-red-50 rounded transition-colors"
      >
        <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16" />
        </svg>
        Delete
      </button>
    </div>
  </div>
</template>

<script setup>
import { computed } from 'vue'

const { todo } = defineProps(['todo'])

defineEmits(['toggle', 'delete', 'edit'])

const statusIcon = computed(() => {
  switch(todo.status) {
    case 'pending': return '⬜'
    case 'in_progress': return '🔄'
    case 'completed': return '✅'
    default: return '⬜'
  }
})
</script>
