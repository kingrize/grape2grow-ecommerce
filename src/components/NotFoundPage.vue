<!-- src/components/NotFoundPage.vue -->
<script>
export default {
  name: 'NotFoundPage',
  methods: {
    goHome() {
      // Mengirim sinyal ke App.vue untuk kembali ke halaman utama
      this.$emit('go-home');
    }
  }
}
</script>

<template>
  <div class="not-found-container">
    <div class="content">
      <h1>404</h1>
      <h2>Halaman Tidak Ditemukan</h2>
      <p>Oops! Sepertinya halaman yang Anda cari tidak ada atau sudah dipindahkan.</p>
      <button @click="goHome" class="home-btn">Kembali ke Beranda</button>
    </div>
  </div>
</template>

<style scoped>
/* src/components/NotFoundPage.vue */
.not-found-container {
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
  padding: 4rem 1rem;
  flex-grow: 1;
}
.content h1 {
  font-size: 8rem;
  font-weight: 700;
  color: var(--primary-color);
  margin: 0;
}
.content h2 {
  font-size: 2rem;
  margin-bottom: 1rem;
  color: var(--secondary-color);
}
.content p {
  color: #666;
  margin-bottom: 2rem;
}
.home-btn {
  background-color: var(--primary-color);
  color: white;
  border: none;
  padding: 0.8rem 1.5rem;
  border-radius: 8px;
  font-size: 1rem;
  font-weight: 600;
  cursor: pointer;
}
</style>
