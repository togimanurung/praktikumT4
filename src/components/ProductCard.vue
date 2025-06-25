<template>
  <div class="product-card">
    <div class="product-content">
      <h3>{{ product.name }}</h3>
      <p class="price">Rp {{ product.price.toLocaleString() }}</p>
      <p class="stock" :class="{ habis: product.stock === 0 }">
        Stok: {{ product.stock }}
      </p>
    </div>
    <button :disabled="product.stock === 0" @click="$emit('buy', product)">
      {{ product.stock === 0 ? 'Habis' : 'Beli' }}
    </button>
  </div>
</template>

<script setup>
defineProps(['product'])
defineEmits(['buy'])
</script>

<style scoped>
.product-card {
  background: linear-gradient(145deg, #ffffff, #f0f0f0);
  border-radius: 16px;
  padding: 20px;
  box-shadow: 0 6px 12px rgba(0,0,0,0.1);
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  transition: transform 0.3s ease;
}

.product-card:hover {
  transform: translateY(-4px);
}

h3 {
  margin-bottom: 8px;
  font-weight: 600;
}

.price {
  color: #2196F3;
  font-weight: bold;
}

.stock {
  font-size: 14px;
  margin-bottom: 12px;
}

.stock.habis {
  color: red;
  font-weight: bold;
}

button {
  padding: 10px;
  background-color: #4CAF50;
  border: none;
  color: white;
  border-radius: 10px;
  cursor: pointer;
  transition: 0.3s;
}

button:hover {
  background-color: #43a047;
}

button:disabled {
  background-color: #aaa;
  cursor: not-allowed;
}
</style>
