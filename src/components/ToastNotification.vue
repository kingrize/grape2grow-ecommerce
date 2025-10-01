<!-- src/components/ToastNotification.vue -->
<script>
import VueFeather from 'vue-feather';

export default {
  name: 'ToastNotification',
  components: {
    VueFeather
  },
  props: {
    message: {
      type: String,
      required: true
    },
    type: {
      type: String,
      default: 'success'
    }
  },
  data() {
    return {
      isVisible: false
    };
  },
  mounted() {
    // Tampilkan notifikasi saat komponen dimuat
    this.isVisible = true;
    // Atur waktu untuk menghilangkannya setelah 4 detik
    setTimeout(() => {
      this.isVisible = false;
    }, 4000);
    // Setelah transisi selesai, kirim sinyal untuk menghapus komponen dari DOM
    setTimeout(() => {
        this.$emit('destroy');
    }, 4500); // 4000ms durasi + 500ms transisi
  }
}
</script>

<template>
  <transition name="toast-fade">
    <div v-if="isVisible" class="toast" :class="`toast-${type}`">
      <vue-feather type="check-circle" class="toast-icon"></vue-feather>
      <span>{{ message }}</span>
    </div>
  </transition>
</template>

<style scoped>
/* src/components/ToastNotification.vue */
.toast {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 1rem;
  border-radius: 8px;
  color: white;
  font-weight: 600;
  box-shadow: 0 4px 12px rgba(0,0,0,0.15);
  margin-bottom: 1rem;
  cursor: default;
}

.toast-success {
  background-color: #2e7d32; /* Warna hijau untuk notifikasi sukses */
}

.toast-icon {
  flex-shrink: 0;
}

/* Transisi untuk animasi masuk dan keluar */
.toast-fade-enter-active,
.toast-fade-leave-active {
  transition: all 0.5s cubic-bezier(0.68, -0.55, 0.27, 1.55);
}

.toast-fade-enter-from,
.toast-fade-leave-to {
  opacity: 0;
  transform: translateX(100%);
}
</style>
