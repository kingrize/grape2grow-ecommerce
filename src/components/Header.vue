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
    // Prop baru untuk menerima pemicu animasi
    trigger: {
      type: Number,
      default: 0
    }
  },
  data() {
    return {
      // State lokal untuk mengontrol kelas animasi
      isAnimating: false
    }
  },
  watch: {
    // Menonton perubahan pada 'trigger'
    trigger() {
      // Jika trigger berubah, mulai animasi
      this.isAnimating = true;

      // Hapus kelas animasi setelah selesai agar bisa dipicu lagi
      setTimeout(() => {
        this.isAnimating = false;
      }, 600); // Durasi harus sama dengan durasi animasi di CSS
    }
  },
  methods: {
    handleToggleCart() {
      this.$emit('toggle-cart');
    }
  }
}
</script>

<template>
  <header class="header">
    <div class="container header-container">
      <div class="logo">
        🌿 Grape2Grow
      </div>
      <nav class="navbar">
        <a href="#" class="active">Beranda</a>
        <a href="#">Produk</a>
        <a href="#">Kontak</a>
      </nav>
      <div class="header-actions">
        <!-- Menambahkan class 'shake' secara dinamis -->
        <button 
          @click="handleToggleCart" 
          class="cart-button" 
          :class="{ 'shake': isAnimating }"
          aria-label="Buka Keranjang">
          <vue-feather type="shopping-cart"></vue-feather>
          <span v-if="cartItemCount > 0" class="cart-badge">{{ cartItemCount }}</span>
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
  padding: 1rem 0;
  box-shadow: 0 2px 4px rgba(0,0,0,0.05);
  position: sticky;
  top: 0;
  z-index: 100;
  width: 100%;
}
.header-container {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.logo {
  font-size: 1.5rem;
  font-weight: 700;
  color: var(--secondary-color);
}
.navbar {
  display: flex;
  gap: 1rem;
}
.navbar a {
  color: #333;
  text-decoration: none;
  font-weight: 600;
  transition: color 0.3s ease;
  position: relative;
  padding: 0.5rem;
}
.navbar a:after {
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
.navbar a:hover:after, .navbar a.active:after {
  width: 100%;
  left: 0;
  background: var(--primary-color);
}
.navbar a:hover, .navbar a.active {
  color: var(--primary-color);
}

.header-actions {
  position: relative;
}

.cart-button {
  background: none;
  border: none;
  cursor: pointer;
  position: relative;
  padding: 8px;
  color: #333;
  transition: color 0.3s ease;
}

.cart-button:hover {
  color: var(--primary-color);
}

.cart-badge {
  position: absolute;
  top: 0;
  right: 0;
  background-color: #e63946;
  color: white;
  width: 18px;
  height: 18px;
  border-radius: 50%;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 0.7rem;
  font-weight: bold;
  transform: scale(1);
  transition: transform 0.2s;
}

/* --- ANIMASI BARU --- */
.cart-button.shake {
  animation: shake 0.6s cubic-bezier(.36,.07,.19,.97) both;
}
.cart-button.shake .cart-badge {
  transform: scale(1.3);
}

@keyframes shake {
  10%, 90% {
    transform: translate3d(-1px, 0, 0);
  }
  20%, 80% {
    transform: translate3d(2px, 0, 0);
  }
  30%, 50%, 70% {
    transform: translate3d(-4px, 0, 0);
  }
  40%, 60% {
    transform: translate3d(4px, 0, 0);
  }
}
/* --- AKHIR ANIMASI BARU --- */


@media (max-width: 768px) {
  .navbar {
    display: none;
  }
}
</style>

