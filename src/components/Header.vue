<!-- src/components/Header.vue -->
<script>
import VueFeather from 'vue-feather';

export default {
  name: 'AppHeader',
  components: {
    VueFeather
  },
  props: {
    cartItemCount: {
      type: Number,
      default: 0
    },
    trigger: Number,
    activeView: String
  },
  data() {
    return {
      isShaking: false
    }
  },
  watch: {
    trigger() {
      this.isShaking = true;
      setTimeout(() => {
        this.isShaking = false;
      }, 600);
    }
  },
  methods: {
    navigate(view) {
      this.$emit('navigate', view);
    },
    toggleCart() {
      this.$emit('toggle-cart');
    },
    // Emit event baru untuk membuka menu mobile
    toggleMobileNav() {
      this.$emit('toggle-mobile-nav');
    }
  }
}
</script>

<template>
  <header class="header">
    <div class="container">
      <div class="logo" @click="navigate('home')">
        🍇 Grape2Grow
      </div>
      
      <!-- Navigasi Desktop (sembunyi di mobile) -->
      <nav class="navbar desktop-nav">
        <a href="#" @click.prevent="navigate('home')" :class="{ active: activeView === 'home' }">Beranda</a>
        <a href="#" @click.prevent="navigate('about')" :class="{ active: activeView === 'about' }">Tentang Kami</a>
        <a href="#" @click.prevent="navigate('contact')" :class="{ active: activeView === 'contact' }">Kontak</a>
      </nav>

      <div class="header-actions">
        <button @click="toggleCart" class="cart-button" :class="{ shake: isShaking }" aria-label="Buka Keranjang">
          <vue-feather type="shopping-cart"></vue-feather>
          <span v-if="cartItemCount > 0" class="cart-badge">{{ cartItemCount }}</span>
        </button>

        <!-- Tombol Hamburger (hanya tampil di mobile) -->
        <button @click="toggleMobileNav" class="mobile-nav-toggle" aria-label="Buka Menu">
          <vue-feather type="menu"></vue-feather>
        </button>
      </div>
    </div>
  </header>
</template>

<style scoped>
/* src/components/Header.vue */
.header {
  background-color: rgba(255, 255, 255, 0.8);
  backdrop-filter: blur(10px);
  -webkit-backdrop-filter: blur(10px);
  color: #333;
  padding: 1rem 0;
  box-shadow: 0 2px 4px rgba(0,0,0,0.05);
  position: sticky;
  top: 0;
  z-index: 100;
  width: 100%;
}
.container {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0 2rem;
}
.logo {
  font-size: 1.5rem;
  font-weight: 700;
  color: var(--secondary-color);
  cursor: pointer;
}
.desktop-nav {
  display: flex;
  gap: 2rem;
}
.desktop-nav a {
  color: #333;
  text-decoration: none;
  font-weight: 600;
  transition: color 0.3s ease;
  position: relative;
  padding-bottom: 5px;
}
.desktop-nav a:after {
  content: '';
  position: absolute;
  width: 0;
  height: 2px;
  display: block;
  margin-top: 5px;
  right: 0;
  background: var(--primary-color);
  transition: width 0.3s ease;
}
.desktop-nav a:hover:after, .desktop-nav a.active:after {
  width: 100%;
  left: 0;
  background: var(--primary-color);
}
.desktop-nav a:hover, .desktop-nav a.active {
  color: var(--primary-color);
}
.header-actions {
  display: flex;
  align-items: center;
  gap: 1rem;
}
.cart-button {
  background: none;
  border: none;
  cursor: pointer;
  position: relative;
  padding: 5px;
  color: #333;
}
.cart-badge {
  position: absolute;
  top: -5px;
  right: -8px;
  background-color: #e63946;
  color: white;
  width: 20px;
  height: 20px;
  border-radius: 50%;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 0.75rem;
  font-weight: bold;
  border: 2px solid white;
}
.shake {
  animation: shake 0.6s cubic-bezier(.36,.07,.19,.97) both;
}
@keyframes shake {
  10%, 90% { transform: translate3d(-1px, 0, 0); }
  20%, 80% { transform: translate3d(2px, 0, 0); }
  30%, 50%, 70% { transform: translate3d(-4px, 0, 0); }
  40%, 60% { transform: translate3d(4px, 0, 0); }
}

/* Tombol Hamburger */
.mobile-nav-toggle {
  display: none; /* Sembunyikan di desktop */
  background: none;
  border: none;
  cursor: pointer;
  padding: 5px;
  color: #333;
}

/* Tampilan Mobile */
@media (max-width: 768px) {
  .desktop-nav {
    display: none; /* Sembunyikan navigasi desktop */
  }
  .mobile-nav-toggle {
    display: block; /* Tampilkan tombol hamburger */
  }
  .container {
    padding: 0 1rem;
  }
}
</style>

