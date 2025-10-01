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
        image: 'https://ik.imagekit.io/notargazyu/Anggur/anggur.jpg', // Nilai default
        bestSeller: false
      },
      copySuccess: false
    };
  },
  computed: {
    generatedCode() {
      const jsonString = JSON.stringify(this.product, null, 2);
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
      <vue-feather type="code" class="header-icon"></vue-feather>
      <h3>Developer Tool: Product Code Generator</h3>
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
          <textarea id="description" v-model="product.description" rows="3" placeholder="cth: Varian impor dengan rasa manis dan adaptif."></textarea>
        </div>
        
        <!-- TAMBAHAN: Input untuk URL Gambar -->
        <div class="form-group">
          <label for="image">URL Gambar</label>
          <input type="text" id="image" v-model="product.image" placeholder="https://contoh.com/gambar.jpg">
        </div>

        <div class="form-group-checkbox">
          <input type="checkbox" id="bestSeller" v-model="product.bestSeller">
          <label for="bestSeller">Tandai sebagai Produk Terlaris (Best Seller)</label>
        </div>
        <div class="form-group">
          <label for="id">ID Produk (unik)</label>
          <div class="id-group">
            <input type="number" id="id" v-model.number="product.id" disabled>
            <button @click="generateNewId" class="id-btn">Generate Baru</button>
          </div>
        </div>
      </div>
      <div class="code-section">
        <h4>Generated Code:</h4>
        <div class="code-container">
          <pre><code>{{ generatedCode }}</code></pre>
          <button @click="copyCode" class="copy-btn">
            <span v-if="!copySuccess">
              <vue-feather type="copy" size="16"></vue-feather> Salin Kode
            </span>
            <span v-else>
              <vue-feather type="check" size="16"></vue-feather> Berhasil!
            </span>
          </button>
        </div>
        <p class="instruction">Salin kode di atas dan tempelkan ke dalam array di file `src/data/products.js`.</p>
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
.dev-tool-body {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1.5rem;
  padding: 1.5rem;
}
.form-group, .form-group-checkbox {
  margin-bottom: 1rem;
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
  display: flex;
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

.code-container {
  position: relative;
  background-color: #1a202c;
  border-radius: 6px;
  border: 1px solid #4a5568;
}
pre {
  padding: 1rem;
  padding-top: 3rem;
  white-space: pre-wrap;
  word-wrap: break-word;
  font-family: 'Courier New', Courier, monospace;
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
  display: flex;
  align-items: center;
  gap: 5px;
}
.instruction {
  margin-top: 1rem;
  font-size: 0.9rem;
  color: #a0aec0;
}
@media (max-width: 992px) {
  .dev-tool-body {
    grid-template-columns: 1fr;
  }
}
</style>

