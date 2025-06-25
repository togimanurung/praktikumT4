<template>
  <div class="container">
    <h1>🛍️ Penjualan Produk</h1>

    <div class="form-card">
      <input v-model="name" placeholder="Nama Produk" />
      <input v-model.number="price" type="number" placeholder="Harga (Rp)" />
      <input v-model.number="stock" type="number" placeholder="Stok Barang" />
      <button @click="addProduct">+ Tambah Produk</button>
    </div>

    <div class="grid">
      <ProductCard
        v-for="product in products"
        :key="product.id"
        :product="product"
        @buy="buyProduct"
      />
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'
import ProductCard from '@/components/ProductCard.vue'

const products = ref([])
const name = ref('')
const price = ref(null)
const stock = ref(null)

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

const buyProduct = async (product) => {
  if (product.stock <= 0) return
  await axios.put(`${API}/${product.id}`, {
    ...product,
    stock: product.stock - 1
  })
  fetchProducts()
}

onMounted(fetchProducts)
</script>

<style scoped>
.container {
  max-width: 1000px;
  margin: 50px auto;
  padding: 20px;
  font-family: 'Segoe UI', sans-serif;
  text-align: center;
  background: #f9f9f9;
  border-radius: 12px;
  box-shadow: 0 10px 25px rgba(0,0,0,0.05);
}

h1 {
  margin-bottom: 30px;
  font-size: 2.4em;
  color: #333;
}

.form-card {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 12px;
  margin-bottom: 30px;
}

input {
  padding: 10px 14px;
  border: 1px solid #ccc;
  border-radius: 10px;
  width: 200px;
  font-size: 14px;
  background-color: #fff;
}

button {
  padding: 10px 16px;
  background-color: #2196F3;
  color: white;
  border: none;
  border-radius: 10px;
  font-weight: bold;
  cursor: pointer;
  transition: background 0.3s ease;
}

button:hover {
  background-color: #1976d2;
}

.grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(260px, 1fr));
  gap: 20px;
}
</style>
