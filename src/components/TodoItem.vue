<template>
  <div class="todo-item">
    <input type="checkbox" :checked="todo.completed" @change="toggleTodo" />
    <span>{{ todo.text }}</span>
    <button @click="$emit('edit', todo)">Edit</button>
    <button @click="$emit('delete', todo.id)">Hapus</button>
  </div>
</template>

<script setup>
defineProps(['todo'])
const emit = defineEmits(['delete', 'edit', 'toggle'])

const toggleTodo = () => {
  emit('toggle', { ...todo, completed: !todo.completed })
}
</script>

<style scoped>
.todo-item {
  margin: 8px 0;
}
</style>
