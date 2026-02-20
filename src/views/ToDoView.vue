<template>
  <div class="space-y-6">
    <!-- Summary Cards -->
    <ToDoSummary :todos="todos" />

    <!-- Filter -->
    <ToDoFilter v-model="filterValue" />

    <!-- List -->
    <ToDoList
      :todos="filteredTodos"
      @toggle="handleToggleStatus"
      @delete="handleDelete"
      @add="openAddModal"
      @edit="openEditModal"
    />

    <!-- Modal -->
    <Modal
      v-model="isModalOpen"
      @save="handleModalSave"
      @cancel="handleModalCancel"
    >
      <template #header>
        {{ modalMode === 'add' ? 'Add New Task' : 'Edit Task' }}
      </template>

      <TodoForm v-model="todoTitle" />
    </Modal>
  </div>
</template>

<script setup>
import { computed, reactive, ref } from 'vue'
import ToDoSummary from '../components/ToDoSummary.vue'
import ToDoFilter from '../components/ToDoFilter.vue'
import ToDoList from '../components/ToDoList.vue'
import Modal from '../components/Modal.vue'
import TodoForm from '../components/TodoForm.vue'

// ========================================
// MODAL STATE
// ========================================

// Controls modal open/close state
const isModalOpen = ref(false)

// Determines modal behavior: 'add' for new todo, 'edit' for existing
const modalMode = ref('add')

// Stores the ID of the todo being edited
const editingTodoId = ref(null)

// Stores the title input value from the form
const todoTitle = ref('')

// ========================================
// DATA
// ========================================

// Reactive array of all todos
const todos = reactive([
  {
    id: 0,
    title: 'Fix header responsive layout',
    status: 'pending'
  },
  {
    id: 1,
    title: 'Add loading spinner component',
    status: 'pending'
  },
  {
    id: 2,
    title: 'Implement user authentication',
    status: 'in_progress'
  },
  {
    id: 3,
    title: 'Write unit tests for API calls',
    status: 'pending'
  },
  {
    id: 4,
    title: 'Optimize images for performance',
    status: 'completed'
  },
  {
    id: 5,
    title: 'Create dark mode toggle',
    status: 'in_progress'
  },
  {
    id: 6,
    title: 'Update dependencies to latest versions',
    status: 'completed'
  },
  {
    id: 7,
    title: 'Add form validation rules',
    status: 'pending'
  },
  {
    id: 8,
    title: 'Deploy to production server',
    status: 'completed'
  },
  {
    id: 9,
    title: 'Review pull requests from team',
    status: 'in_progress'
  }
])

// Stores the current filter text
const filterValue = ref('')

// ========================================
// COMPUTED
// ========================================

/**
 * Returns filtered todos based on search query
 * If filterValue is empty, returns all todos
 * Otherwise filters by title (case-insensitive)
 */
const filteredTodos = computed(() => {
  if (filterValue.value === '') {
    return todos
  } else {
    return todos.filter((todo) => {
      // Safety check: skip if todo or title is undefined
      if (!todo || !todo.title) return false

      return todo.title.toLowerCase().includes(filterValue.value.toLowerCase())
    })
  }
})

// ========================================
// TODO CRUD METHODS
// ========================================

/**
 * Toggles todo status in cycle: pending → in_progress → completed → pending
 * @param {number} id - The ID of the todo to toggle
 */
const handleToggleStatus = (id) => {
  const todo = todos.find((todo) => todo.id === id)

  if (!todo) return

  if (todo.status === 'pending') {
    todo.status = 'in_progress'
  } else if (todo.status === 'in_progress') {
    todo.status = 'completed'
  } else if (todo.status === 'completed') {
    todo.status = 'pending'
  }
}

/**
 * Deletes a todo from the list
 * @param {number} id - The ID of the todo to delete
 */
const handleDelete = (id) => {
  const index = todos.findIndex((todo) => todo.id === id)

  if (index !== -1) {
    todos.splice(index, 1)
  }
}

/**
 * Adds a new todo to the list with status 'pending'
 * Generates new ID by incrementing the maximum existing ID
 * @param {string} title - The title of the new todo
 */
const handleAdd = (title) => {
  const maxId = Math.max(...todos.map(t => t.id))

  const newTodo = {
    id: maxId + 1,
    title: title,
    status: 'pending'
  }

  todos.push(newTodo)
}

/**
 * Updates the title of an existing todo
 * @param {number} id - The ID of the todo to edit
 * @param {string} newTitle - The new title value
 */
const handleEdit = (id, newTitle) => {
  const todo = todos.find((todo) => todo.id === id)

  if (!todo) return

  todo.title = newTitle
}

// ========================================
// MODAL METHODS
// ========================================

/**
 * Opens modal in 'add' mode with empty form
 * Triggered when user clicks "New Item" button
 */
const openAddModal = () => {
  modalMode.value = 'add'
  todoTitle.value = ''
  editingTodoId.value = null
  isModalOpen.value = true
}

/**
 * Opens modal in 'edit' mode with pre-filled form
 * Triggered when user clicks "Edit" button on a todo
 * @param {number} id - The ID of the todo to edit
 */
const openEditModal = (id) => {
  modalMode.value = 'edit'

  // Find the todo and populate form with its current title
  const todo = todos.find(t => t.id === id)
  if (todo) {
    todoTitle.value = todo.title
    editingTodoId.value = id
  }

  isModalOpen.value = true
}

/**
 * Saves todo based on current modal mode
 * In 'add' mode: creates new todo
 * In 'edit' mode: updates existing todo
 * Validates that title is not empty before saving
 */
const handleModalSave = () => {
  // Validate: title must not be empty
  if (!todoTitle.value.trim()) {
    alert('Please enter a task title')
    return
  }

  if (modalMode.value === 'add') {
    // Add new todo
    handleAdd(todoTitle.value)
  } else {
    // Edit existing todo
    handleEdit(editingTodoId.value, todoTitle.value)
  }

  // Close modal and reset form
  isModalOpen.value = false
  todoTitle.value = ''
}

/**
 * Cancels modal operation and resets form
 * Triggered when user clicks "Cancel" or closes modal
 */
const handleModalCancel = () => {
  isModalOpen.value = false
  todoTitle.value = ''
  editingTodoId.value = null
}
</script>
