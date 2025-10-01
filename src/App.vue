<!-- src/App.vue -->
<script>
import productsData from './data/products.js';
import Header from './components/Header.vue';
import ProductCard from './components/ProductCard.vue';
import Footer from './components/Footer.vue';
import ShoppingCart from './components/ShoppingCart.vue';
import DeveloperTool from './components/DeveloperTool.vue';

export default {
  name: 'App',
  components: {
    Header,
    ProductCard,
    Footer,
    ShoppingCart,
    DeveloperTool
  },
  data() {
    return {
      products: productsData,
      cart: [], // 1. Mulai dengan keranjang kosong, akan diisi dari localStorage
      isCartOpen: false,
      cartAnimationTrigger: 0,
      showDevTools: false
    };
  },
  computed: {
    totalCartItems() {
      return this.cart.reduce((total, item) => total + item.quantity, 0);
    }
  },
  // 2. Gunakan 'created' lifecycle hook untuk memuat data saat aplikasi dimulai
  created() {
    // Muat keranjang dari localStorage
    const savedCart = localStorage.getItem('grape2grow_cart');
    if (savedCart) {
      try {
        // Coba parse data JSON
        this.cart = JSON.parse(savedCart);
      } catch (e) {
        console.error("Gagal memuat keranjang dari localStorage:", e);
        // Hapus data yang rusak agar tidak menyebabkan error lagi
        localStorage.removeItem('grape2grow_cart');
      }
    }
  },
  watch: {
    isCartOpen(newValue) {
      if (newValue) {
        document.body.classList.add('no-scroll');
      } else {
        document.body.classList.remove('no-scroll');
      }
    },
    // 3. Tambahkan 'watcher' untuk memantau perubahan pada keranjang
    cart: {
      // Fungsi 'handler' akan berjalan setiap kali 'cart' berubah
      handler(newCart) {
        // Simpan versi terbaru dari keranjang ke localStorage
        localStorage.setItem('grape2grow_cart', JSON.stringify(newCart));
      },
      deep: true // 'deep' penting agar perubahan di dalam objek (cth: kuantitas) juga terdeteksi
    }
  },
  mounted() {
    const urlParams = new URLSearchParams(window.location.search);
    if (urlParams.get('dev') === 'true') {
      this.showDevTools = true;
    }
  },
  methods: {
    handleAddToCart(product) {
      const existingItem = this.cart.find(item => item.id === product.id);
      if (existingItem) {
        if (existingItem.quantity < existingItem.stock) {
          existingItem.quantity++;
        }
      } else {
        this.cart.push({ ...product, quantity: 1 });
      }
      this.cartAnimationTrigger++;
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
    handleClearCart() {
      this.cart = [];
    },
    toggleCart() {
      this.isCartOpen = !this.isCartOpen;
    }
  }
}
</script>

<template>
  <div id="app-wrapper">
    <Header 
      :cartItemCount="totalCartItems"
      :trigger="cartAnimationTrigger"
      @toggle-cart="toggleCart"
    />
    <main class="container">
      <h2 class="section-title">Produk Bibit Anggur Pilihan</h2>
      <p class="section-subtitle">Kualitas terbaik untuk kebun Anda. Tambahkan ke keranjang sekarang!</p>
      
      <DeveloperTool v-if="showDevTools" />
      
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
      @clear-cart="handleClearCart"
      @close-cart="toggleCart"
    />
    <div v-if="isCartOpen" @click="toggleCart" class="cart-overlay"></div>
  </div>
</template>

<style>
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

