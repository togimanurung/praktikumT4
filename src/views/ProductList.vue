<template>
  <div class="container">
    <h1>🛒 Tambah Produk</h1>

    <div class="form">
      <input v-model="name" placeholder="Nama Produk" />
      <input v-model.number="price" type="number" placeholder="Harga (Rp)" />
      <input v-model.number="stock" type="number" placeholder="Stok" />
      <button @click="addProduct">+ Tambah</button>
    </div>

    <h2>📦 Daftar Produk</h2>
    <div class="product-list">
      <div v-for="product in products" :key="product.id" class="card">
        <h3>{{ product.name }}</h3>
        <p>Harga: Rp {{ product.price.toLocaleString() }}</p>
        <p>Stok: {{ product.stock }}</p>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'

const name = ref('')
const price = ref(null)
const stock = ref(null)
const products = ref([])

const API = 'http://localhost:3000/products'

const fetchProducts = async () => {
  const res = await axios.get(API)
  products.value = res.data
}

const addProduct = async () => {
  if (!name.value || !price.value || !stock.value) return

  await axios.post(API, {
    name: name.value,
    price: price.value,
    stock: stock.value
  })

  name.value = ''
  price.value = null
  stock.value = null

  fetchProducts()
}

onMounted(fetchProducts)
</script>

<style scoped>
.container {
  max-width: 800px;
  margin: 40px auto;
  font-family: sans-serif;
  text-align: center;
  padding: 20px;
}

.form {
  display: flex;
  gap: 10px;
  flex-wrap: wrap;
  justify-content: center;
  margin-bottom: 30px;
}

input {
  padding: 10px;
  border-radius: 8px;
  border: 1px solid #ccc;
  width: 200px;
}

button {
  padding: 10px 20px;
  background: #4CAF50;
  color: white;
  border: none;
  border-radius: 8px;
  cursor: pointer;
}

button:hover {
  background: #388e3c;
}

.product-list {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(240px, 1fr));
  gap: 20px;
  padding: 10px;
}

.card {
  background: #f5f5f5;
  padding: 16px;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0,0,0,0.05);
}
</style>
