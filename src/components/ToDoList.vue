<template>
  <div class="bg-white rounded-lg shadow-sm overflow-hidden">

    <div class="flex items-center justify-between p-4 border-b border-gray-200">
      <div class="flex gap-8">
        <h2 class="text-sm font-semibold text-gray-700 uppercase">Status</h2>
        <h2 class="text-sm font-semibold text-gray-700 uppercase">Item</h2>
      </div>

      <!-- New Item Button -->
      <button
        @click="$emit('add')"
        class="flex items-center gap-2 px-4 py-2 bg-blue-500 text-white rounded-lg hover:bg-blue-600 transition-colors"
      >
        <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 4v16m8-8H4" />
        </svg>
        New Item
      </button>
    </div>

    <!-- List Items -->
    <div class="divide-y divide-gray-200">
      <ToDoItem
        v-for="todo in todos"
        :key="todo.id"
        :todo="todo"
        @toggle="$emit('toggle', $event)"
        @delete="$emit('delete', $event)"
        @edit="$emit('edit', $event)"
      />
    </div>
  </div>
</template>

<script setup>
import ToDoItem from './ToDoItem.vue'

defineProps(['todos'])
defineEmits(['toggle', 'delete', 'edit', 'add'])
</script>
