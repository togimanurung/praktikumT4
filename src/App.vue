<template>
  <div class="container">
    <h1>Todo List dengan Axios & JSON Server</h1>

    <div class="form">
      <input v-model="newTodo" placeholder="Tambah todo baru..." />
      <button @click="addTodo">Tambah</button>
    </div>

    <ul>
      <li v-for="todo in todos" :key="todo.id">
        {{ todo.text }}
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'

const todos = ref([])
const newTodo = ref('')
const apiUrl = 'http://localhost:3000/todos'

// Ambil data dari JSON Server saat halaman dimuat
const fetchTodos = async () => {
  const res = await axios.get(apiUrl)
  todos.value = res.data
}

// Tambahkan todo baru
const addTodo = async () => {
  if (newTodo.value.trim() === '') return

  await axios.post(apiUrl, {
    text: newTodo.value.trim()
  })

  newTodo.value = ''
  fetchTodos()
}

// Jalankan saat halaman pertama kali muncul
onMounted(() => {
  fetchTodos()
})
</script>

<style scoped>
.container {
  max-width: 600px;
  margin: 40px auto;
  text-align: center;
  font-family: sans-serif;
}

h1 {
  margin-bottom: 20px;
}

.form {
  margin-bottom: 20px;
}

input {
  padding: 8px;
  width: 60%;
  border: 1px solid #ccc;
}

button {
  padding: 8px 12px;
  margin-left: 10px;
  background-color: #34a853;
  color: white;
  border: none;
  cursor: pointer;
}

ul {
  text-align: left;
  padding: 0;
  list-style: none;
}

li {
  background: #f2f2f2;
  margin: 8px 0;
  padding: 10px;
  border-radius: 4px;
}
</style>
