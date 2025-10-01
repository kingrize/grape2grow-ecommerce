<!-- src/App.vue -->
<script>
import productsData from './data/products.js';
import Header from './components/Header.vue';
import ProductCard from './components/ProductCard.vue';
import Footer from './components/Footer.vue';
import ShoppingCart from './components/ShoppingCart.vue';
import DeveloperTool from './components/DeveloperTool.vue';
// 1. Impor komponen detail
import ProductDetail from './components/ProductDetail.vue';

export default {
  name: 'App',
  // 2. Daftarkan komponen detail
  components: {
    Header,
    ProductCard,
    Footer,
    ShoppingCart,
    DeveloperTool,
    ProductDetail
  },
  data() {
    return {
      products: productsData,
      cart: [],
      isCartOpen: false,
      cartAnimationTrigger: 0,
      showDevTools: false,
      // 3. State baru untuk routing
      currentView: 'list', // 'list' atau 'detail'
      selectedProduct: null // Untuk menyimpan produk yang sedang dilihat
    };
  },
  computed: {
    totalCartItems() {
      return this.cart.reduce((total, item) => total + item.quantity, 0);
    }
  },
  created() {
    const savedCart = localStorage.getItem('grape2grow_cart');
    if (savedCart) {
      try {
        this.cart = JSON.parse(savedCart);
      } catch (e) {
        console.error("Gagal memuat keranjang dari localStorage:", e);
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
    cart: {
      handler(newCart) {
        localStorage.setItem('grape2grow_cart', JSON.stringify(newCart));
      },
      deep: true
    }
  },
  mounted() {
    const urlParams = new URLSearchParams(window.location.search);
    if (urlParams.get('dev') === 'true') {
      this.showDevTools = true;
    }
  },
  methods: {
    // 4. Method baru untuk menampilkan detail produk
    viewProductDetails(product) {
      this.selectedProduct = product;
      this.currentView = 'detail';
      window.scrollTo(0, 0); // Scroll ke atas halaman
    },
    // 5. Method untuk kembali ke daftar produk
    backToList() {
      this.selectedProduct = null;
      this.currentView = 'list';
    },
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
      <!-- 6. Gunakan v-if dan v-else untuk beralih tampilan -->
      <div v-if="currentView === 'list'">
        <h2 class="section-title">Produk Bibit Anggur Pilihan</h2>
        <p class="section-subtitle">Kualitas terbaik untuk kebun Anda. Tambahkan ke keranjang sekarang!</p>
        <DeveloperTool v-if="showDevTools" />
        <div class="product-grid">
          <ProductCard
            v-for="product in products"
            :key="product.id"
            :product="product"
            @add-to-cart="handleAddToCart"
            @view-details="viewProductDetails"
          />
        </div>
      </div>
      
      <div v-else-if="currentView === 'detail' && selectedProduct">
        <ProductDetail 
          :product="selectedProduct"
          @back-to-list="backToList"
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
/* (Tidak ada perubahan di style, biarkan seperti semula) */
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

