<template>
  <div class="min-h-screen bg-gray-50">
    <MainHeader />
    <main class="container mx-auto px-4 py-6">
      <ToDoView />
    </main>
  </div>
</template>

<script setup>
import MainHeader from './components/MainHeader.vue'
import ToDoView from './views/ToDoView.vue'
</script>
