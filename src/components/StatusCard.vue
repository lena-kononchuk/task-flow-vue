<template>
  <div
    class="rounded-lg p-6 shadow-sm"
    :class="cardColorClass"
  >
    <h3 class="text-sm font-medium text-gray-700 mb-2">
      {{ title }}
    </h3>
    <p class="text-4xl font-bold text-gray-900">
      {{ count }}
    </p>
  </div>
</template>

<script setup>
import { computed } from 'vue'

// Props
const { title, count, color } = defineProps({
  title: {
    type: String,
    required: true
  },
  count: {
    type: Number,
    required: true
  },
  color: {
    type: String,
    required: true
  }
})


const cardColorClass = computed(() => {
  const colorMap = {
    gray: 'bg-gray-200',
    teal: 'bg-teal-500 text-white',
    green: 'bg-green-500 text-white'
  }
  return colorMap[color] || 'bg-gray-200'
})
</script>
