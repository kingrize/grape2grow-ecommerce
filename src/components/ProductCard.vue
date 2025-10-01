<!-- src/components/ProductCard.vue -->
<script>
import VueFeather from 'vue-feather';

export default {
  name: 'ProductCard',
  components: {
    VueFeather
  },
  props: {
    product: {
      type: Object,
      required: true
    }
  },
  methods: {
    addToCart() {
      if (this.product.stock === 0) return;
      this.$emit('add-to-cart', this.product);
    },
    // Method baru untuk mengirim sinyal lihat detail
    viewDetails() {
      this.$emit('view-details', this.product);
    }
  }
}
</script>

<template>
  <!-- Menambahkan event click di wrapper utama -->
  <div class="product-card" :class="{ 'out-of-stock': product.stock === 0 }" @click="viewDetails">
    <div class="product-image-container">
      <div v-if="product.bestSeller" class="bestseller-label">
        <vue-feather type="star" class="bestseller-icon"></vue-feather>
        <span>Terlaris</span>
      </div>
      <div v-if="product.stock === 0" class="sold-out-overlay">
        <span>Habis Terjual</span>
      </div>
      <img :src="product.image" :alt="product.name" class="product-image">
    </div>
    <div class="product-info">
      <h3 class="product-name">{{ product.name }}</h3>
      <p class="product-description">{{ product.description }}</p>
      <p class="product-price">Rp {{ product.price.toLocaleString('id-ID') }}</p>
      
      <!-- Hentikan event click agar tidak bubble up ke parent saat tombol ini diklik -->
      <button 
        @click.stop="addToCart" 
        class="buy-btn"
        :disabled="product.stock === 0"
        :class="{ 'disabled': product.stock === 0 }">
        <span v-if="product.stock > 0" class="btn-content">
          <vue-feather type="plus" class="btn-icon"></vue-feather>
          Tambah ke Keranjang
        </span>
        <span v-else>
          Stok Habis
        </span>
      </button>
    </div>
  </div>
</template>

<style scoped>
/* src/components/ProductCard.vue */
.product-card {
  /* ... (semua style lain tetap sama) ... */
  cursor: pointer; /* Tambahkan ini untuk menandakan bisa diklik */
}
/* ... (sisa style tidak berubah, biarkan seperti semula) ... */
.product-card {
  background-color: var(--card-bg);
  border-radius: 12px;
  overflow: hidden;
  box-shadow: 0 4px 15px rgba(0,0,0,0.05);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
  display: flex;
  flex-direction: column;
  position: relative;
}
.product-card:hover {
  transform: translateY(-8px);
  box-shadow: 0 10px 25px rgba(0,0,0,0.1);
}
.product-image-container {
  width: 100%;
  padding-top: 75%;
  position: relative;
  overflow: hidden;
}
.product-image {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.4s ease;
}
.product-card:hover .product-image {
  transform: scale(1.05);
}
.product-info {
  padding: 1.25rem;
  text-align: center;
  flex-grow: 1;
  display: flex;
  flex-direction: column;
}
.product-name {
  margin-bottom: 0.5rem;
  font-size: 1.1rem;
  font-weight: 600;
}
.product-description {
  font-size: 0.9rem;
  color: #666;
  margin-bottom: 1rem;
  flex-grow: 1;
}
.product-price {
  color: #555;
  margin-bottom: 1rem;
  font-size: 1rem;
  font-weight: 600;
}
.buy-btn {
  background: linear-gradient(45deg, var(--primary-color), #81C784);
  color: white;
  border: none;
  padding: 0.75rem 1rem;
  border-radius: 8px;
  cursor: pointer;
  font-size: 0.95rem;
  font-weight: 600;
  transition: transform 0.2s ease, box-shadow 0.2s ease, background-color 0.3s;
  width: 100%;
}
.btn-content {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 8px;
}
.buy-btn:hover:not(:disabled) {
  transform: scale(1.05);
  box-shadow: 0 4px 12px rgba(76, 175, 80, 0.4);
}
.buy-btn.disabled {
  background: #ccc;
  cursor: not-allowed;
}
.bestseller-label {
  position: absolute;
  top: 10px;
  right: 10px;
  background-color: #ffffff;
  color: #333;
  padding: 5px 10px;
  border-radius: 20px;
  font-size: 0.8rem;
  font-weight: 600;
  z-index: 2;
  box-shadow: 0 2px 5px rgba(0,0,0,0.2);
  display: flex;
  align-items: center;
  gap: 5px;
  border: 1px solid #ffc107;
}
.bestseller-icon {
  width: 16px;
  height: 16px;
  color: #e6a200;
}
.btn-icon {
  width: 18px;
  height: 18px;
}
.sold-out-overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  color: white;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 1.2rem;
  font-weight: 700;
  z-index: 1;
}
.product-card.out-of-stock .product-image {
  filter: grayscale(80%);
}
</style>

