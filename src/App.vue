<!-- src/App.vue -->
<script>
import productsData from './data/products.js';
import Header from './components/Header.vue';
import ProductCard from './components/ProductCard.vue';
import Footer from './components/Footer.vue';
import ShoppingCart from './components/ShoppingCart.vue';
import DeveloperTool from './components/DeveloperTool.vue';
import ProductDetail from './components/ProductDetail.vue';
// 1. Hapus impor CategoryFilter dan ganti dengan SearchBar
import SearchBar from './components/SearchBar.vue';

export default {
  name: 'App',
  // 2. Daftarkan SearchBar dan hapus CategoryFilter
  components: {
    Header,
    ProductCard,
    Footer,
    ShoppingCart,
    DeveloperTool,
    ProductDetail,
    SearchBar
  },
  data() {
    return {
      products: productsData,
      cart: [],
      isCartOpen: false,
      cartAnimationTrigger: 0,
      showDevTools: false,
      currentView: 'list',
      selectedProduct: null,
      // 3. Ganti state kategori dengan state untuk pencarian
      searchQuery: ''
    };
  },
  computed: {
    totalCartItems() {
      return this.cart.reduce((total, item) => total + item.quantity, 0);
    },
    // 4. Ganti logika `uniqueCategories` dan `filteredProducts`
    //    dengan satu computed property untuk hasil pencarian
    filteredProducts() {
      // Jika kotak pencarian kosong, tampilkan semua produk
      if (!this.searchQuery) {
        return this.products;
      }

      // Ubah query pencarian ke huruf kecil untuk pencarian case-insensitive
      const query = this.searchQuery.toLowerCase();
      
      // Filter produk berdasarkan nama, deskripsi, kategori, dan tags
      return this.products.filter(p => {
        const inName = p.name.toLowerCase().includes(query);
        const inDescription = p.description && p.description.toLowerCase().includes(query);
        const inCategory = p.category && p.category.toLowerCase().includes(query);
        const inTags = p.tags && p.tags.some(tag => tag.toLowerCase().includes(query));
        
        return inName || inDescription || inCategory || inTags;
      });
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
    // 5. Hapus method handleFilterCategory
    viewProductDetails(product) {
      this.selectedProduct = product;
      this.currentView = 'detail';
      window.scrollTo(0, 0);
    },
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
      <div v-if="currentView === 'list'">
        <h2 class="section-title">Produk Bibit Anggur Pilihan</h2>
        <p class="section-subtitle">Kualitas terbaik untuk kebun Anda. Tambahkan ke keranjang sekarang!</p>
        
        <!-- 6. Ganti CategoryFilter dengan SearchBar -->
        <SearchBar v-model="searchQuery" />

        <DeveloperTool v-if="showDevTools" />
        
        <div class="product-grid">
          <ProductCard
            v-for="product in filteredProducts"
            :key="product.id"
            :product="product"
            @add-to-cart="handleAddToCart"
            @view-details="viewProductDetails"
          />
        </div>
        
        <!-- Tambahkan pesan jika tidak ada hasil pencarian -->
        <div v-if="filteredProducts.length === 0" class="no-results">
          <h3>Oops! Produk tidak ditemukan.</h3>
          <p>Coba gunakan kata kunci lain untuk mencari produk yang Anda inginkan.</p>
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
/* ... (style global tidak berubah) ... */
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

/* Style baru untuk pesan "tidak ada hasil" */
.no-results {
  text-align: center;
  padding: 4rem 1rem;
  color: #888;
}
.no-results h3 {
  font-size: 1.5rem;
  margin-bottom: 0.5rem;
  color: #555;
}
</style>

