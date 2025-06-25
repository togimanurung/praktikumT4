<template>
  <div class="container">
    <h1>Todo List dengan Axios & JSON Server (CRUD lengkap)</h1>
    
    <div class="input-section">
      <input v-model="newTodo" placeholder="Tambah todo baru..." />
      <button @click="addTodo">Tambah</button>
    </div>

    <div v-for="todo in todos" :key="todo.id">
      <TodoItem
        :todo="todo"
        @delete="deleteTodo"
        @edit="editTodo"
        @toggle="updateTodo"
      />
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'
import TodoItem from '@/components/TodoItem.vue'

const todos = ref([])
const newTodo = ref('')
const api = 'http://localhost:3000/todos'

const fetchTodos = async () => {
  const res = await axios.get(api)
  todos.value = res.data
}

const addTodo = async () => {
  if (!newTodo.value) return
  await axios.post(api, { text: newTodo.value, completed: false })
  newTodo.value = ''
  fetchTodos()
}

const deleteTodo = async (id) => {
  await axios.delete(`${api}/${id}`)
  fetchTodos()
}

const updateTodo = async (todo) => {
  await axios.put(`${api}/${todo.id}`, todo)
  fetchTodos()
}

const editTodo = (todo) => {
  const newText = prompt('Edit todo:', todo.text)
  if (newText !== null) {
    updateTodo({ ...todo, text: newText })
  }
}

onMounted(fetchTodos)
</script>

<style scoped>
.container {
  max-width: 700px;
  margin: 50px auto;
  text-align: center;
}

.input-section {
  margin-bottom: 20px;
}

input[type="text"] {
  padding: 8px;
  width: 60%;
  border: 1px solid #ccc;
}

button {
  margin-left: 10px;
  padding: 8px 12px;
  background-color: #eee;
  border: none;
  cursor: pointer;
}
</style>
