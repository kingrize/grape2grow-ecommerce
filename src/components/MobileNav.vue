<!-- src/components/MobileNav.vue -->
<script>
import VueFeather from 'vue-feather';

export default {
  name: 'MobileNav',
  components: {
    VueFeather
  },
  props: {
    isOpen: Boolean,
    activeView: String
  },
  emits: ['navigate', 'close-nav'],
  methods: {
    // Method untuk navigasi sekaligus menutup menu
    navigateAndClose(view) {
      this.$emit('navigate', view);
      this.$emit('close-nav');
    }
  }
}
</script>

<template>
  <transition name="fade">
    <div v-if="isOpen" class="mobile-nav-wrapper">
      <!-- Overlay gelap di belakang -->
      <div class="overlay" @click="$emit('close-nav')"></div>
      
      <!-- Panel Menu -->
      <nav class="nav-panel">
        <div class="panel-header">
          <h3>Menu</h3>
          <button @click="$emit('close-nav')" class="close-btn" aria-label="Tutup Menu">
            <vue-feather type="x"></vue-feather>
          </button>
        </div>
        <div class="panel-links">
          <a href="#" @click.prevent="navigateAndClose('home')" :class="{ active: activeView === 'home' }">Beranda</a>
          <a href="#" @click.prevent="navigateAndClose('about')" :class="{ active: activeView === 'about' }">Tentang Kami</a>
          <a href="#" @click.prevent="navigateAndClose('contact')" :class="{ active: activeView === 'contact' }">Kontak</a>
        </div>
      </nav>
    </div>
  </transition>
</template>

<style scoped>
/* src/components/MobileNav.vue */
.mobile-nav-wrapper {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 2000; /* Di atas segalanya */
}

.overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0,0,0,0.5);
}

.nav-panel {
  position: absolute;
  top: 0;
  right: 0;
  width: 80%;
  max-width: 300px;
  height: 100%;
  background-color: white;
  box-shadow: -4px 0 15px rgba(0,0,0,0.1);
  transform: translateX(0);
  transition: transform 0.4s cubic-bezier(0.25, 0.46, 0.45, 0.94);
}

.panel-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem 1.5rem;
  border-bottom: 1px solid #eee;
}
.panel-header h3 {
  font-size: 1.2rem;
}
.close-btn {
  background: none;
  border: none;
  cursor: pointer;
  padding: 5px;
}

.panel-links {
  display: flex;
  flex-direction: column;
  padding: 1rem;
}
.panel-links a {
  padding: 1rem;
  text-decoration: none;
  color: #333;
  font-weight: 600;
  font-size: 1.1rem;
  border-radius: 8px;
  transition: background-color 0.2s, color 0.2s;
}
.panel-links a:hover {
  background-color: #f0f0f0;
}
.panel-links a.active {
  background-color: var(--primary-color);
  color: white;
}

/* Animasi untuk overlay dan panel */
.fade-enter-active, .fade-leave-active {
  transition: opacity 0.4s ease;
}
.fade-enter-from, .fade-leave-to {
  opacity: 0;
}
.fade-enter-active .nav-panel, .fade-leave-active .nav-panel {
  transition: transform 0.4s cubic-bezier(0.25, 0.46, 0.45, 0.94);
}
.fade-enter-from .nav-panel, .fade-leave-to .nav-panel {
  transform: translateX(100%);
}
</style>
