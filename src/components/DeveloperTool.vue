<!-- src/components/DeveloperTool.vue -->
<script>
import VueFeather from 'vue-feather';

export default {
  name: 'DeveloperTool',
  components: {
    VueFeather
  },
  data() {
    return {
      product: {
        id: Math.floor(100 + Math.random() * 900),
        name: '',
        price: 0,
        stock: 10,
        description: '',
        // Kolom baru untuk data yang lebih detail
        longDescription: '',
        specs: {
            height: '',
            age: '',
            climate: ''
        },
        category: '',
        image: 'https://ik.imagekit.io/notargazyu/Anggur/anggur.jpg',
        bestSeller: false,
      },
      tagsString: '', // Input terpisah untuk tags agar mudah diolah
      copySuccess: false,
      isCodeVisible: false
    };
  },
  computed: {
    generatedCode() {
      // Buat salinan objek produk untuk dimanipulasi
      const productToGenerate = { ...this.product };

      // Olah string tags menjadi array
      productToGenerate.tags = this.tagsString
        .split(',')
        .map(tag => tag.trim())
        .filter(tag => tag !== '');

      const jsonString = JSON.stringify(productToGenerate, null, 2);
      return `${jsonString},`;
    }
  },
  methods: {
    generateNewId() {
      this.product.id = Math.floor(100 + Math.random() * 900);
    },
    copyCode() {
      navigator.clipboard.writeText(this.generatedCode).then(() => {
        this.copySuccess = true;
        setTimeout(() => {
          this.copySuccess = false;
        }, 2000);
      }).catch(err => {
        console.error('Gagal menyalin kode: ', err);
      });
    }
  }
}
</script>

<template>
  <div class="dev-tool-wrapper">
    <div class="dev-tool-header">
      <vue-feather type="tool" class="header-icon"></vue-feather>
      <h3>Developer Tool</h3>
    </div>

    <div class="dev-tool-body">
      <div class="form-section">
        <div class="form-group">
          <label for="name">Nama Produk</label>
          <input type="text" id="name" v-model="product.name" placeholder="cth: Bibit Anggur Jupiter">
        </div>
        <div class="form-group-row">
          <div class="form-group">
            <label for="price">Harga (Rp)</label>
            <input type="number" id="price" v-model.number="product.price">
          </div>
          <div class="form-group">
            <label for="stock">Stok</label>
            <input type="number" id="stock" v-model.number="product.stock">
          </div>
        </div>
        <div class="form-group">
          <label for="description">Deskripsi Singkat</label>
          <textarea id="description" v-model="product.description" rows="2" placeholder="Teks singkat untuk di kartu produk."></textarea>
        </div>
        <div class="form-group">
          <label for="longDescription">Deskripsi Lengkap</label>
          <textarea id="longDescription" v-model="product.longDescription" rows="4" placeholder="Deskripsi detail untuk halaman produk."></textarea>
        </div>
        
        <fieldset class="specs-group">
          <legend>Spesifikasi</legend>
          <div class="form-group-row">
            <div class="form-group">
              <label for="spec-height">Tinggi Bibit (cm)</label>
              <input type="text" id="spec-height" v-model="product.specs.height" placeholder="cth: 30-50">
            </div>
            <div class="form-group">
              <label for="spec-age">Usia Bibit (bulan)</label>
              <input type="text" id="spec-age" v-model="product.specs.age" placeholder="cth: 3-4">
            </div>
          </div>
           <div class="form-group">
              <label for="spec-climate">Iklim Tumbuh</label>
              <input type="text" id="spec-climate" v-model="product.specs.climate" placeholder="cth: Dataran rendah - tinggi">
            </div>
        </fieldset>

        <div class="form-group-row">
          <div class="form-group">
            <label for="category">Kategori</label>
            <input type="text" id="category" v-model="product.category" placeholder="cth: Anggur Meja">
          </div>
          <div class="form-group">
            <label for="tags">Tags / Kata Kunci</label>
            <input type="text" id="tags" v-model="tagsString" placeholder="impor, manis, tanpa biji">
          </div>
        </div>

        <div class="form-group">
          <label for="image">URL Gambar</label>
          <input type="text" id="image" v-model="product.image" placeholder="https://contoh.com/gambar.jpg">
        </div>

        <div class="form-group-checkbox">
          <input type="checkbox" id="bestSeller" v-model="product.bestSeller">
          <label for="bestSeller">Tandai sebagai Produk Terlaris</label>
        </div>
        <div class="form-group">
          <label for="id">ID Produk (unik)</label>
          <div class="id-group">
            <input type="number" id="id" v-model.number="product.id" readonly>
            <button @click="generateNewId" class="id-btn">Generate Baru</button>
          </div>
        </div>
      </div>

      <!-- Tombol ini HANYA TAMPIL di mobile/tablet untuk membuka/tutup area kode -->
      <button @click="isCodeVisible = !isCodeVisible" class="toggle-code-btn">
        <span class="btn-content">
          <vue-feather :type="isCodeVisible ? 'chevron-up' : 'chevron-down'" size="18"></vue-feather>
          {{ isCodeVisible ? 'Sembunyikan Kode' : 'Tampilkan Generated Code' }}
        </span>
      </button>

      <!-- Area kode ini akan expand/collapse di mobile, dan selalu tampil di desktop -->
      <div class="code-section" :class="{ 'is-visible': isCodeVisible }">
        <h4>Generated Code:</h4>
        <div class="code-container">
          <pre><code>{{ generatedCode }}</code></pre>
          <button @click="copyCode" class="copy-btn">
            <span v-if="!copySuccess" class="copy-btn-content">
              <vue-feather type="copy" size="16"></vue-feather> Salin
            </span>
            <span v-else class="copy-btn-content">
              <vue-feather type="check" size="16"></vue-feather> Berhasil!
            </span>
          </button>
        </div>
        <p class="instruction">Salin dan tempel kode ini ke dalam file `src/data/products.js`.</p>
      </div>
    </div>
  </div>
</template>

<style scoped>
/* src/components/DeveloperTool.vue */
.dev-tool-wrapper {
  background-color: #2d3748;
  color: #e2e8f0;
  border-radius: 12px;
  margin: 3rem 0;
  border: 1px solid #4a5568;
  overflow: hidden;
}
.dev-tool-header {
  padding: 1rem 1.5rem;
  border-bottom: 1px solid #4a5568;
  display: flex;
  align-items: center;
  gap: 10px;
}
.dev-tool-header .header-icon {
  color: #63b3ed;
}
.dev-tool-header h3 {
  margin: 0;
  font-size: 1.2rem;
}

/* Default: 2 kolom untuk layar lebar */
.dev-tool-body {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1.5rem;
  padding: 1.5rem;
}
.form-group, .form-group-checkbox {
  margin-bottom: 1rem;
}
.form-group:last-child {
  margin-bottom: 0;
}
.form-group label {
  display: block;
  margin-bottom: 0.5rem;
  font-weight: 600;
  font-size: 0.9rem;
}
.form-group input, .form-group textarea {
  width: 100%;
  padding: 0.75rem;
  border-radius: 6px;
  border: 1px solid #4a5568;
  background-color: #1a202c;
  color: #e2e8f0;
  font-size: 1rem;
}
.form-group-row {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1rem;
}
.form-group-checkbox {
  display: flex;
  align-items: center;
  gap: 0.5rem;
}
.id-group {
  display: flex;
  gap: 0.5rem;
}
.id-group input {
  background-color: #4a5568;
  cursor: not-allowed;
}
.id-btn {
  padding: 0.75rem;
  border: none;
  background-color: #63b3ed;
  color: #1a202c;
  font-weight: bold;
  border-radius: 6px;
  cursor: pointer;
}
.specs-group {
  border: 1px solid #4a5568;
  padding: 1rem;
  padding-top: 0.5rem;
  border-radius: 6px;
  margin-bottom: 1rem;
}
.specs-group legend {
  padding: 0 0.5rem;
  font-weight: 600;
  font-size: 0.9rem;
}

.code-container {
  position: relative;
  background-color: #1a202c;
  border-radius: 6px;
  border: 1px solid #4a5568;
  min-height: 250px;
  display: flex;
  flex-direction: column;
}
pre {
  padding: 1rem;
  padding-top: 3.5rem;
  font-family: 'Courier New', Courier, monospace;
  white-space: pre-wrap;
  word-break: break-all;
  flex-grow: 1;
  overflow-y: auto;
  box-sizing: border-box;
}

.copy-btn {
  position: absolute;
  top: 10px;
  right: 10px;
  background-color: #4a5568;
  color: #e2e8f0;
  border: none;
  padding: 0.5rem 0.75rem;
  border-radius: 6px;
  cursor: pointer;
  z-index: 2;
}
.copy-btn-content {
  display: flex;
  align-items: center;
  gap: 5px;
}
.instruction {
  margin-top: 1rem;
  font-size: 0.9rem;
  color: #a0aec0;
}
.toggle-code-btn {
  display: none; /* Sembunyikan tombol ini di desktop */
}

@media (max-width: 1023px) {
  .dev-tool-body {
    grid-template-columns: 1fr; /* Tampilan satu kolom */
    padding: 1.5rem;
  }
  .toggle-code-btn {
    display: flex; /* Tampilkan tombol di mobile */
    justify-content: center;
    align-items: center;
    width: 100%;
    padding: 0.8rem;
    margin-top: 1rem;
    border: 1px solid #4a5568;
    background-color: #1a202c;
    color: #a0aec0;
    font-weight: 600;
    border-radius: 8px;
    cursor: pointer;
    transition: background-color 0.2s;
  }
  .toggle-code-btn:hover {
    background-color: #2d3748;
  }
  .btn-content {
    display: flex;
    align-items: center;
    gap: 8px;
  }
  
  .code-section {
    display: none;
    margin-top: 1.5rem;
  }
  .code-section.is-visible {
    display: block;
  }
  
  @media (min-width: 1024px) {
    .code-section {
      display: block !important;
      margin-top: 0;
    }
  }
}

@media (max-width: 576px) {
  .dev-tool-body {
    padding: 1rem;
  }
  .form-group-row {
    grid-template-columns: 1fr;
  }
  pre {
    padding: 0.75rem;
    padding-top: 3.5rem;
  }
}
</style>

