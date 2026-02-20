<template>
  <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
    <!-- Pending Card -->
    <StatusCard
      title="Pending"
      :count="pendingCount"
      color="gray"
    />

    <!-- In Progress Card -->
    <StatusCard
      title="In progress"
      :count="inProgressCount"
      color="teal"
    />

    <!-- Completed Card -->
    <StatusCard
      title="Completed"
      :count="completedCount"
      color="green"
    />
  </div>
</template>

<script setup>
import { computed } from 'vue'
import StatusCard from './StatusCard.vue'

const props = defineProps(['todos'])

const pendingCount = computed(() => {
  return props.todos.filter(todo => todo.status === 'pending').length
})

const inProgressCount = computed(() => {
  return props.todos.filter(todo => todo.status === 'in_progress').length
})

const completedCount = computed(() => {
  return props.todos.filter(todo => todo.status === 'completed').length
})
</script>
