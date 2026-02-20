<template>
  <div class="space-y-4">
    <!-- Label -->
    <div>
      <label
        for="todo-title"
        class="block text-sm font-medium text-gray-700 mb-2"
      >
        Task Title
      </label>

      <!-- Input -->
      <input
        id="todo-title"
        type="text"
        :value="modelValue"
        @input="$emit('update:modelValue', $event.target.value)"
        placeholder="Enter task title..."
        class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent"
      />
    </div>

    <p class="text-sm text-gray-500">
      Enter a clear and concise task description
    </p>
  </div>
</template>

<script setup>
// Props для v-model
defineProps({
  modelValue: {
    type: String,
    default: ''
  }
})


defineEmits(['update:modelValue'])
</script>
