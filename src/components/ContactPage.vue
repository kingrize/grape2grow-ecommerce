<!-- src/components/ContactPage.vue -->
<script>
import VueFeather from 'vue-feather';

export default {
  name: 'ContactPage',
  components: {
    VueFeather
  }
}
</script>

<template>
  <div class="contact-container">
    <div class="header-section">
      <h1>Hubungi Kami</h1>
      <p>Kami siap membantu menjawab pertanyaan Anda seputar produk dan pemesanan.</p>
    </div>

    <div class="contact-layout">
      <div class="contact-info">
        <h3>Informasi Kontak</h3>
        <div class="info-item">
          <vue-feather type="map-pin" class="info-icon"></vue-feather>
          <div>
            <strong>Alamat Kebun:</strong>
            <p>
              Gg. Hj. Uman, Kp. Kebon Kopi, RT.002/RW.001<br>
              Desa Sukadami, Kec. Cikarang Selatan<br>
              Kab. Bekasi, Jawa Barat 17532
            </p>
          </div>
        </div>
        <div class="info-item">
          <vue-feather type="phone" class="info-icon"></vue-feather>
          <div>
            <strong>WhatsApp (Pemesanan):</strong>
            <p>+62 851-7528-0710</p>
          </div>
        </div>
        <div class="info-item">
          <vue-feather type="mail" class="info-icon"></vue-feather>
          <div>
            <strong>Email:</strong>
            <p>argasystem@gmail.com</p>
          </div>
        </div>
        <div class="social-media">
          <h4>Ikuti Kami</h4>
          <a href="https://www.tiktok.com/@g_grape2grow" target="_blank" rel="noopener noreferrer" class="social-link" aria-label="TikTok">
            <!-- SVG Ikon TikTok -->
            <svg class="tiktok-icon" viewBox="0 0 24 24" fill="currentColor" width="24" height="24">
              <path d="M12.525.02c1.31-.02 2.61-.01 3.91-.02.08 1.53.63 3.09 1.75 4.17 1.12 1.11 2.7 1.62 4.24 1.79v4.03c-1.44-.05-2.89-.35-4.2-.97-.57-.26-1.1-.59-1.62-.93-.01 2.92.01 5.84-.02 8.75-.08 1.4-.54 2.79-1.35 3.94-1.31 1.92-3.58 3.17-5.91 3.21-2.43.03-4.83-.95-6.43-2.98-1.55-1.99-2.3-4.52-2.1-7.1.13-1.21.43-2.38.89-3.48.48-1.13 1.14-2.14 1.96-3.01.62-.65 1.34-1.2 2.12-1.66.45-.26.93-.46 1.44-.62 1.53-.48 3.09-.76 4.65-.92Z"></path>
            </svg>
            <span>g_grape2grow</span>
          </a>
        </div>
      </div>
      <div class="map-placeholder">
        <p>Lokasi Peta Akan Ditampilkan Di Sini</p>
      </div>
    </div>
  </div>
</template>

<style scoped>
/* src/components/ContactPage.vue */
.contact-container {
  animation: fadeIn 0.5s ease-out;
}
@keyframes fadeIn {
  from { opacity: 0; transform: translateY(10px); }
  to { opacity: 1; transform: translateY(0); }
}

.header-section {
  text-align: center;
  margin-bottom: 3rem;
}
.header-section h1 {
  font-size: 2.8rem;
  color: var(--secondary-color);
  margin-bottom: 0.5rem;
}
.header-section p {
  font-size: 1.2rem;
  color: #666;
}

.contact-layout {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 3rem;
  background: #fff;
  padding: 3rem;
  border-radius: 12px;
  border: 1px solid #eee;
}

.contact-info h3 {
  font-size: 1.8rem;
  margin-bottom: 2rem;
  border-bottom: 2px solid var(--primary-color);
  padding-bottom: 0.5rem;
  display: inline-block;
}

.info-item {
  display: flex;
  align-items: flex-start;
  gap: 1rem;
  margin-bottom: 1.5rem;
}
.info-icon {
  color: var(--primary-color);
  flex-shrink: 0;
  margin-top: 5px;
}
.info-item strong {
  display: block;
  margin-bottom: 0.25rem;
}
.info-item p {
  margin: 0;
  line-height: 1.6;
  color: #555;
}

.social-media {
  margin-top: 2rem;
}
.social-link {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  color: var(--secondary-color);
  text-decoration: none;
  font-weight: 600;
  transition: color 0.2s;
}
.social-link:hover {
  color: var(--primary-color);
}
.tiktok-icon {
  width: 24px;
  height: 24px;
}

.map-placeholder {
  background-color: #f0f0f0;
  border-radius: 12px;
  display: flex;
  justify-content: center;
  align-items: center;
  font-weight: bold;
  color: #aaa;
  min-height: 300px;
}

@media (max-width: 992px) {
  .contact-layout {
    grid-template-columns: 1fr;
    padding: 2rem;
  }
}
</style>

