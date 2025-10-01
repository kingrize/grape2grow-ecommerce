<!-- src/App.vue -->
<script>
// Impor data dan semua komponen yang dibutuhkan
import productsData from './data/products.js';
import Header from './components/Header.vue';
import ProductCard from './components/ProductCard.vue';
import Footer from './components/Footer.vue';
import ShoppingCart from './components/ShoppingCart.vue';

export default {
  name: 'App',
  components: {
    Header,
    ProductCard,
    Footer,
    ShoppingCart
  },
  data() {
    return {
      products: productsData,
      cart: [],
      isCartOpen: false,
      // Properti baru untuk memicu animasi di Header
      cartAnimationTrigger: 0
    };
  },
  computed: {
    // Menghitung total item untuk badge di header
    totalCartItems() {
      return this.cart.reduce((total, item) => total + item.quantity, 0);
    }
  },
  watch: {
    isCartOpen(newValue) {
      if (newValue) {
        document.body.classList.add('no-scroll');
      } else {
        document.body.classList.remove('no-scroll');
      }
    }
  },
  methods: {
    // Fungsi untuk menangani event 'add-to-cart'
    handleAddToCart(product) {
      const existingItem = this.cart.find(item => item.id === product.id);
      if (existingItem) {
        if (existingItem.quantity < existingItem.stock) {
          existingItem.quantity++;
        }
      } else {
        this.cart.push({ ...product, quantity: 1 });
      }
      
      // PERUBAHAN: Tidak lagi membuka keranjang, tapi memicu animasi
      // this.isCartOpen = true; // Baris ini dihapus
      this.cartAnimationTrigger++; // Baris ini ditambahkan
    },
    
    handleUpdateQuantity(product, newQuantity) {
      const itemInCart = this.cart.find(item => item.id === product.id);
      if (itemInCart && newQuantity <= itemInCart.stock) {
        itemInCart.quantity = newQuantity;
      }
    },
    
    handleRemoveItem(product) {
      this.cart = this.cart.filter(item => item.id !== product.id);
    },

    toggleCart() {
      this.isCartOpen = !this.isCartOpen;
    }
  }
}
</script>

<template>
  <div id="app-wrapper">
    <!-- Mengirim prop 'trigger' baru ke Header -->
    <Header 
      :cartItemCount="totalCartItems"
      :trigger="cartAnimationTrigger"
      @toggle-cart="toggleCart"
    />
    <main class="container">
      <h2 class="section-title">Produk Bibit Anggur Pilihan</h2>
      <p class="section-subtitle">Kualitas terbaik untuk kebun Anda. Tambahkan ke keranjang sekarang!</p>
      <div class="product-grid">
        <ProductCard
          v-for="product in products"
          :key="product.id"
          :product="product"
          @add-to-cart="handleAddToCart"
        />
      </div>
    </main>
    <Footer />
    
    <ShoppingCart 
      :cartItems="cart" 
      :class="{ 'is-open': isCartOpen }"
      @update-quantity="handleUpdateQuantity"
      @remove-item="handleRemoveItem"
      @close-cart="toggleCart"
    />
    
    <div v-if="isCartOpen" @click="toggleCart" class="cart-overlay"></div>
  </div>
</template>

<style>
/* Sebaiknya, pindahkan style ini ke file src/style.css Anda */
.no-scroll {
  overflow: hidden;
}
#app-wrapper {
  transition: none;
}
.cart-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0,0,0,0.5);
  z-index: 999;
  transition: opacity 0.3s ease;
}
</style>

