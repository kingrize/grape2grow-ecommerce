<!-- src/components/ProductDetail.vue -->
<script>
import VueFeather from 'vue-feather';

export default {
  name: 'ProductDetail',
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
    goBack() {
      // Mengirim sinyal kembali ke App.vue untuk kembali ke daftar produk
      this.$emit('back-to-list');
    },
    addToCart() {
      // Mengirim sinyal untuk menambahkan produk ini ke keranjang
      this.$emit('add-to-cart', this.product);
    }
  }
}
</script>

<template>
  <div class="product-detail-container">
    <button @click="goBack" class="back-btn">
      <vue-feather type="arrow-left" size="18"></vue-feather>
      Kembali ke Produk
    </button>

    <div class="detail-layout">
      <!-- Kolom Gambar -->
      <div class="image-gallery">
        <img :src="product.image" :alt="product.name" class="main-image">
      </div>

      <!-- Kolom Informasi -->
      <div class="product-info">
        <span v-if="product.category" class="category-tag">{{ product.category }}</span>
        <h1 class="product-title">{{ product.name }}</h1>
        <p class="product-price">Rp {{ product.price.toLocaleString('id-ID') }}</p>
        
        <p class="short-description">{{ product.description }}</p>

        <div class="stock-info">
          <span v-if="product.stock > 0" class="in-stock">Stok Tersedia: {{ product.stock }}</span>
          <span v-else class="out-of-stock">Stok Habis</span>
        </div>

        <button @click="addToCart" class="add-to-cart-btn" :disabled="product.stock === 0">
          <vue-feather type="plus" size="20"></vue-feather>
          Tambah ke Keranjang
        </button>

        <div class="specs-section">
          <h4>Spesifikasi Bibit</h4>
          <ul>
            <li v-if="product.specs?.height">
              <vue-feather type="bar-chart-2" size="16"></vue-feather>
              <strong>Tinggi:</strong> {{ product.specs.height }} cm
            </li>
            <li v-if="product.specs?.age">
              <vue-feather type="clock" size="16"></vue-feather>
              <strong>Usia:</strong> {{ product.specs.age }} bulan
            </li>
            <li v-if="product.specs?.climate">
              <vue-feather type="sun" size="16"></vue-feather>
              <strong>Iklim:</strong> {{ product.specs.climate }}
            </li>
          </ul>
        </div>
      </div>
    </div>

    <!-- Deskripsi Lengkap di Bawah -->
    <div class="long-description-section" v-if="product.longDescription">
      <h3>Deskripsi Lengkap</h3>
      <p>{{ product.longDescription }}</p>
    </div>
    
    <div class="tags-section" v-if="product.tags && product.tags.length > 0">
      <strong>Tags:</strong>
      <span v-for="tag in product.tags" :key="tag" class="tag">{{ tag }}</span>
    </div>

  </div>
</template>

<style scoped>
/* src/components/ProductDetail.vue */
.product-detail-container {
  padding: 2rem 0;
  animation: fadeIn 0.5s ease-out;
}
@keyframes fadeIn {
  from { opacity: 0; transform: translateY(10px); }
  to { opacity: 1; transform: translateY(0); }
}

.back-btn {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  background: none;
  border: 1px solid #ddd;
  padding: 0.5rem 1rem;
  border-radius: 8px;
  cursor: pointer;
  margin-bottom: 2rem;
  font-weight: 600;
}

.detail-layout {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 3rem;
}

.image-gallery .main-image {
  width: 100%;
  border-radius: 12px;
  box-shadow: 0 10px 20px rgba(0,0,0,0.1);
}

.category-tag {
  background-color: var(--primary-color);
  color: white;
  padding: 4px 10px;
  border-radius: 20px;
  font-size: 0.8rem;
  font-weight: bold;
}

.product-title {
  font-size: 2.5rem;
  margin: 1rem 0;
}

.product-price {
  font-size: 1.8rem;
  font-weight: 700;
  color: var(--secondary-color);
  margin-bottom: 1.5rem;
}

.short-description {
  font-size: 1rem;
  color: #666;
  line-height: 1.6;
  margin-bottom: 1.5rem;
}

.stock-info {
  margin-bottom: 1.5rem;
  font-weight: 600;
}
.in-stock { color: #2e7d32; }
.out-of-stock { color: #c62828; }

.add-to-cart-btn {
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 10px;
  padding: 1rem;
  font-size: 1.1rem;
  font-weight: 600;
  color: white;
  background-color: var(--primary-color);
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: background-color 0.2s;
}
.add-to-cart-btn:hover:not(:disabled) {
  background-color: var(--secondary-color);
}
.add-to-cart-btn:disabled {
  background-color: #ccc;
  cursor: not-allowed;
}

.specs-section {
  margin-top: 2rem;
  padding-top: 2rem;
  border-top: 1px solid #eee;
}
.specs-section h4 {
  margin-bottom: 1rem;
}
.specs-section ul {
  list-style: none;
  padding: 0;
}
.specs-section li {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 0.75rem;
  color: #333;
}
.specs-section li strong {
  width: 80px;
}

.long-description-section {
  margin-top: 3rem;
  padding-top: 2rem;
  border-top: 1px solid #eee;
}
.long-description-section p {
  line-height: 1.8;
  color: #555;
}

.tags-section {
  margin-top: 2rem;
}
.tag {
  display: inline-block;
  background-color: #f0f0f0;
  padding: 4px 12px;
  border-radius: 15px;
  margin-right: 8px;
  margin-top: 8px;
  font-size: 0.9rem;
}

/* --- PERBAIKAN RESPONSIVE UNTUK MOBILE --- */
@media (max-width: 992px) {
  .detail-layout {
    /* Tumpuk kolom secara vertikal */
    grid-template-columns: 1fr;
    gap: 2rem; /* Kurangi jarak antar elemen */
  }
  .product-title {
    font-size: 2rem; /* Kecilkan ukuran judul */
  }
  .product-price {
    font-size: 1.5rem; /* Kecilkan ukuran harga */
  }
  .image-gallery {
    /* Pastikan gambar tidak terlalu besar di mobile */
    max-width: 500px;
    margin: 0 auto;
  }
}

@media (max-width: 576px) {
  .product-detail-container {
    padding: 1rem 0; /* Kurangi padding di layar sangat kecil */
  }
  .back-btn {
    margin-bottom: 1.5rem;
  }
  .product-title {
    font-size: 1.8rem;
  }
}
</style>

