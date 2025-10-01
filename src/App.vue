<!-- src/App.vue -->
<script>
// ... (script Anda tidak berubah, biarkan seperti semula) ...
import productsData from './data/products.js';
import Header from './components/Header.vue';
import ProductCard from './components/ProductCard.vue';
import Footer from './components/Footer.vue';
import ShoppingCart from './components/ShoppingCart.vue';
import DeveloperTool from './components/DeveloperTool.vue';
import ProductDetail from './components/ProductDetail.vue';
import SearchBar from './components/SearchBar.vue';
import ToastNotification from './components/ToastNotification.vue';
import FilterSort from './components/FilterSort.vue';
import AboutPage from './components/AboutPage.vue';
import ContactPage from './components/ContactPage.vue';
import NotFoundPage from './components/NotFoundPage.vue';
import MobileNav from './components/MobileNav.vue';

export default {
  name: 'App',
  components: {
    Header,
    ProductCard,
    Footer,
    ShoppingCart,
    DeveloperTool,
    ProductDetail,
    SearchBar,
    ToastNotification,
    FilterSort,
    AboutPage,
    ContactPage,
    NotFoundPage,
    MobileNav
  },
  data() {
    return {
      products: productsData,
      cart: [],
      isCartOpen: false,
      cartAnimationTrigger: 0,
      showDevTools: false,
      currentView: 'home',
      selectedProduct: null,
      searchQuery: '',
      toasts: [],
      sortBy: 'default',
      isMobileNavOpen: false
    };
  },
  computed: {
    totalCartItems() {
      return this.cart.reduce((total, item) => total + item.quantity, 0);
    },
    filteredProducts() {
      let processedProducts = [...this.products];
      if (this.searchQuery) {
        const query = this.searchQuery.toLowerCase();
        processedProducts = processedProducts.filter(p => {
          const inName = p.name.toLowerCase().includes(query);
          const inDescription = p.description && p.description.toLowerCase().includes(query);
          const inCategory = p.category && p.category.toLowerCase().includes(query);
          const inTags = p.tags && p.tags.some(tag => tag.toLowerCase().includes(query));
          return inName || inDescription || inCategory || inTags;
        });
      }
      switch (this.sortBy) {
        case 'price-asc':
          processedProducts.sort((a, b) => a.price - b.price);
          break;
        case 'price-desc':
          processedProducts.sort((a, b) => b.price - a.price);
          break;
        case 'name-asc':
          processedProducts.sort((a, b) => a.name.localeCompare(b.name));
          break;
        case 'name-desc':
          processedProducts.sort((a, b) => b.name.localeCompare(a.name));
          break;
      }
      return processedProducts;
    },
    isOverlayActive() {
      return this.isCartOpen || this.isMobileNavOpen;
    }
  },
  created() {
    const savedCart = localStorage.getItem('grape2grow_cart');
    if (savedCart) {
      try { this.cart = JSON.parse(savedCart); } catch (e) {
        console.error("Gagal memuat keranjang:", e);
        localStorage.removeItem('grape2grow_cart');
      }
    }
  },
  watch: {
    isOverlayActive(isActive) {
      document.body.classList.toggle('no-scroll', isActive);
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
    this.showDevTools = urlParams.get('dev') === 'true';
  },
  methods: {
    toggleMobileNav() {
      this.isMobileNavOpen = !this.isMobileNavOpen;
    },
    handleNavigate(view) {
      this.currentView = view;
      window.scrollTo(0, 0);
    },
    viewProductDetails(product) {
      this.selectedProduct = product;
      this.currentView = 'detail';
      window.scrollTo(0, 0);
    },
    backToList() {
      this.selectedProduct = null;
      this.currentView = 'home';
    },
    handleAddToCart(product) {
      const existingItem = this.cart.find(item => item.id === product.id);
      if (existingItem) {
        if (existingItem.quantity < existingItem.stock) existingItem.quantity++;
      } else {
        this.cart.push({ ...product, quantity: 1 });
      }
      this.cartAnimationTrigger++;
      this.showToast(`"${product.name}" berhasil ditambahkan!`);
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
    handleClearCart() { this.cart = []; },
    toggleCart() { this.isCartOpen = !this.isCartOpen; },
    showToast(message, type = 'success') {
        const id = Date.now();
        this.toasts.push({ id, message, type });
    },
    removeToast(toastId) {
        this.toasts = this.toasts.filter(toast => toast.id !== toastId);
    }
  }
}
</script>

<template>
  <div id="app-wrapper">
    <Header 
      :cartItemCount="totalCartItems"
      :trigger="cartAnimationTrigger"
      :activeView="currentView"
      @toggle-cart="toggleCart"
      @navigate="handleNavigate"
      @toggle-mobile-nav="toggleMobileNav"
    />
    <main class="container">
      <div v-if="currentView === 'home'">
        
        <!-- BAGIAN HERO SECTION BARU -->
        <div class="hero-section">
          <div class="hero-content">
            <h1>Wujudkan Kebun Anggur Impian Anda</h1>
            <p>Koleksi bibit anggur impor dan lokal kualitas premium untuk hasil panen terbaik.</p>
            <a href="#product-list" class="cta-button">Lihat Koleksi</a>
          </div>
        </div>

        <h2 class="section-title">Produk Pilihan Kami</h2>
        
        <div class="search-and-filter-bar">
          <SearchBar v-model="searchQuery" />
          <FilterSort v-model="sortBy" />
        </div>

        <DeveloperTool v-if="showDevTools" />
        
        <!-- Tambahkan id agar tombol CTA bisa scroll ke sini -->
        <div id="product-list" class="product-grid">
          <ProductCard
            v-for="product in filteredProducts"
            :key="product.id"
            :product="product"
            @add-to-cart="handleAddToCart"
            @view-details="viewProductDetails"
          />
        </div>
        
        <div v-if="filteredProducts.length === 0" class="no-results">
          <h3>Oops! Produk tidak ditemukan.</h3>
          <p>Coba gunakan kata kunci lain untuk mencari produk yang Anda inginkan.</p>
        </div>
      </div>
      
      <ProductDetail 
        v-else-if="currentView === 'detail' && selectedProduct"
        :product="selectedProduct"
        @back-to-list="backToList"
        @add-to-cart="handleAddToCart"
      />

      <AboutPage v-else-if="currentView === 'about'" />
      <ContactPage v-else-if="currentView === 'contact'" />

      <NotFoundPage v-else @go-home="handleNavigate('home')" />

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
    <MobileNav 
      :isOpen="isMobileNavOpen"
      :activeView="currentView"
      @navigate="handleNavigate"
      @close-nav="isMobileNavOpen = false"
    />

    <div v-if="isCartOpen" @click="toggleCart" class="cart-overlay"></div>
    <div class="toast-container">
      <ToastNotification 
        v-for="toast in toasts"
        :key="toast.id"
        :message="toast.message"
        :type="toast.type"
        @destroy="removeToast(toast.id)"
      />
    </div>
  </div>
</template>

<style>
/* ... (style global lainnya tidak berubah) ... */

/* TAMBAHKAN STYLE BARU INI KE BAGIAN BAWAH */
.hero-section {
  position: relative;
  background-image: linear-gradient(rgba(0,0,0,0.5), rgba(0,0,0,0.5)), url('https://cdn.mos.cms.futurecdn.net/v2/t:65,l:0,cw:1254,ch:705,q:80,w:1254/YT9HWuDKPjSEJdHMYnYR8K.jpg');
  background-size: cover;
  background-position: center;
  color: white;
  text-align: center;
  padding: 6rem 2rem;
  border-radius: 12px;
  margin-bottom: 3rem;
  display: flex;
  justify-content: center;
  align-items: center;
}

.hero-content h1 {
  font-size: 3rem;
  font-weight: 700;
  margin-bottom: 1rem;
  text-shadow: 2px 2px 8px rgba(0,0,0,0.7);
}

.hero-content p {
  font-size: 1.25rem;
  max-width: 600px;
  margin: 0 auto 2rem auto;
  text-shadow: 1px 1px 4px rgba(0,0,0,0.7);
}

.cta-button {
  background-color: var(--primary-color);
  color: white;
  padding: 0.8rem 2rem;
  border-radius: 8px;
  text-decoration: none;
  font-weight: 600;
  font-size: 1.1rem;
  transition: transform 0.2s, background-color 0.2s;
  display: inline-block;
}

.cta-button:hover {
  background-color: var(--secondary-color);
  transform: scale(1.05);
}

@media (max-width: 768px) {
  .hero-section {
    padding: 4rem 1.5rem;
  }
  .hero-content h1 {
    font-size: 2.2rem;
  }
  .hero-content p {
    font-size: 1.1rem;
  }
}

.no-scroll { overflow: hidden; }
#app-wrapper { transition: none; }
.cart-overlay {
  position: fixed; top: 0; left: 0; width: 100%; height: 100%;
  background-color: rgba(0,0,0,0.5); z-index: 999; transition: opacity 0.3s ease;
}
.no-results {
  text-align: center; padding: 4rem 1rem; color: #888;
}
.no-results h3 { font-size: 1.5rem; margin-bottom: 0.5rem; color: #555; }
.toast-container { position: fixed; bottom: 2rem; right: 2rem; z-index: 9999; }
.search-and-filter-bar {
  display: flex; gap: 1rem; align-items: stretch; margin-bottom: 2.5rem;
}
.search-and-filter-bar > :first-child { flex-grow: 1; }
@media (max-width: 768px) {
  .search-and-filter-bar { flex-direction: column; align-items: stretch; margin-bottom: 2rem; }
}
@media (max-width: 576px) {
    .toast-container { bottom: 1rem; right: 1rem; left: 1rem; }
}
</style>

