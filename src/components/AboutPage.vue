<!-- src/components/AboutPage.vue -->
<script>
import VueFeather from 'vue-feather';

export default {
  name: 'AboutPage',
  components: {
    VueFeather
  }
}
</script>

<template>
  <div class="about-container">
    <div class="hero-section">
      <h1>Tentang Grape2Grow</h1>
      <p>Menghadirkan bibit anggur kualitas terbaik dari seluruh dunia langsung ke kebun Anda.</p>
    </div>

    <div class="content-section">
      <div class="mission-vision">
        <div class="card">
          <vue-feather type="target" size="40" class="card-icon"></vue-feather>
          <h3>Misi Kami</h3>
          <p>Misi kami adalah untuk mempermudah para penghobi dan petani di Indonesia untuk mendapatkan akses ke bibit anggur varietas unggul yang terjamin kualitas dan keasliannya, disertai dengan edukasi untuk mencapai hasil panen terbaik.</p>
        </div>
        <div class="card">
          <vue-feather type="eye" size="40" class="card-icon"></vue-feather>
          <h3>Visi Kami</h3>
          <p>Menjadi pusat referensi dan penyedia bibit anggur terpercaya nomor satu di Indonesia, yang dikenal karena koleksi varietasnya yang lengkap, kualitas produk yang superior, dan pelayanan pelanggan yang prima.</p>
        </div>
      </div>

      <div class="story-section">
        <h2>Cerita Kami</h2>
        <p>Grape2Grow berawal dari kecintaan kami terhadap tanaman anggur dan keinginan untuk melihat lebih banyak kebun anggur yang subur di seluruh nusantara. Kami memulai sebagai kolektor, mencoba berbagai varietas dari berbagai negara. Dari pengalaman tersebut, kami belajar betapa sulitnya mendapatkan bibit yang benar-benar berkualitas dan sesuai dengan deskripsinya.</p>
        <p>Oleh karena itu, Grape2Grow lahir. Kami mendedikasikan diri untuk melakukan seleksi ketat, memastikan setiap bibit yang kami jual adalah yang terbaik. Kami percaya bahwa setiap orang berhak merasakan kebahagiaan memanen anggur dari halaman rumahnya sendiri.</p>
      </div>
    </div>
  </div>
</template>

<style scoped>
/* src/components/AboutPage.vue */
.about-container {
  animation: fadeIn 0.5s ease-out;
}
@keyframes fadeIn {
  from { opacity: 0; transform: translateY(10px); }
  to { opacity: 1; transform: translateY(0); }
}

.hero-section {
  text-align: center;
  padding: 4rem 1rem;
  background-color: #f7f9f7;
  border-radius: 12px;
  margin-bottom: 3rem;
}
.hero-section h1 {
  font-size: 2.8rem;
  color: var(--secondary-color);
  margin-bottom: 0.5rem;
}
.hero-section p {
  font-size: 1.2rem;
  color: #666;
  max-width: 600px;
  margin: 0 auto;
}

.mission-vision {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 2rem;
  margin-bottom: 3rem;
}
.card {
  background-color: #fff;
  padding: 2rem;
  border-radius: 12px;
  text-align: center;
  border: 1px solid #eee;
}
.card-icon {
  color: var(--primary-color);
  margin-bottom: 1rem;
}
.card h3 {
  margin-bottom: 0.5rem;
  font-size: 1.5rem;
}

.story-section h2 {
  text-align: center;
  font-size: 2rem;
  margin-bottom: 1.5rem;
  color: var(--secondary-color);
}
.story-section p {
  line-height: 1.8;
  color: #555;
  margin-bottom: 1rem;
}

@media (max-width: 768px) {
  .hero-section h1 {
    font-size: 2.2rem;
  }
  .mission-vision {
    grid-template-columns: 1fr;
  }
}
</style>
