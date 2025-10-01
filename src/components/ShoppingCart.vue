<!-- src/components/ShoppingCart.vue -->
<script>
import VueFeather from 'vue-feather';

export default {
  name: 'ShoppingCart',
  components: {
    VueFeather
  },
  // 1. Tambahkan state baru untuk mengontrol visibilitas modal
  data() {
    return {
      showClearCartConfirm: false
    }
  },
  props: {
    cartItems: {
      type: Array,
      required: true
    }
  },
  computed: {
    totalPrice() {
      return this.cartItems.reduce((total, item) => total + (item.price * item.quantity), 0);
    }
  },
  methods: {
    updateQuantity(product, newQuantity) {
      if (newQuantity > 0) {
        this.$emit('update-quantity', product, newQuantity);
      } else {
        this.$emit('remove-item', product);
      }
    },
    handleCloseCart() {
      this.showClearCartConfirm = false; // Pastikan modal tertutup jika keranjang ditutup
      this.$emit('close-cart');
    },
    // 2. Method ini sekarang hanya akan menampilkan modal
    requestClearCart() {
      this.showClearCartConfirm = true;
    },
    // 3. Method baru untuk benar-benar menghapus setelah konfirmasi
    confirmClearCart() {
      this.$emit('clear-cart');
      this.showClearCartConfirm = false;
    },
    // 4. Method untuk membatalkan
    cancelClearCart() {
      this.showClearCartConfirm = false;
    },
    checkout() {
      if (this.cartItems.length === 0) return;
      const phoneNumber = '6285175280710';
      let message = "Halo, saya ingin memesan produk berikut:\n\n";
      this.cartItems.forEach(item => {
        message += `*${item.name}*\n`;
        message += `Jumlah: ${item.quantity}\n`;
        message += `Subtotal: Rp ${ (item.price * item.quantity).toLocaleString('id-ID') }\n\n`;
      });
      message += `*TOTAL PESANAN: Rp ${this.totalPrice.toLocaleString('id-ID')}*`;
      const encodedMessage = encodeURIComponent(message);
      const whatsappURL = `https://wa.me/${phoneNumber}?text=${encodedMessage}`;
      window.open(whatsappURL, '_blank');
    }
  }
}
</script>

<template>
  <div class="cart-panel">
    <div class="cart-header">
      <h3 class="cart-title">
        <vue-feather type="shopping-cart"></vue-feather>
        Keranjang Saya
      </h3>
      <!-- 5. Ubah event @click ke method baru -->
      <button v-if="cartItems.length > 0" @click="requestClearCart" class="clear-cart-btn" title="Hapus Semua Keranjang">
        <vue-feather type="trash-2" size="20"></vue-feather>
      </button>
      <button @click="handleCloseCart" class="close-btn" aria-label="Tutup Keranjang">
        <vue-feather type="x"></vue-feather>
      </button>
    </div>
    
    <div class="cart-body">
      <!-- ... (Konten cart-body tidak berubah) ... -->
      <div v-if="cartItems.length === 0" class="cart-empty">
        <vue-feather type="slash" size="48" class="empty-icon"></vue-feather>
        <p>Keranjang Anda masih kosong.</p>
        <span>Silakan pilih produk yang Anda inginkan.</span>
      </div>
      <div v-else class="cart-items-list">
        <div v-for="item in cartItems" :key="item.id" class="cart-item">
          <img :src="item.image" :alt="item.name" class="item-image">
          <div class="item-details">
            <p class="item-name">{{ item.name }}</p>
            <p class="item-price">Rp {{ item.price.toLocaleString('id-ID') }}</p>
            <div class="item-quantity">
              <button @click="updateQuantity(item, item.quantity - 1)" class="quantity-btn">-</button>
              <span>{{ item.quantity }}</span>
              <button @click="updateQuantity(item, item.quantity + 1)" :disabled="item.quantity >= item.stock" class="quantity-btn">+</button>
            </div>
          </div>
        </div>
      </div>
    </div>
    
    <div v-if="cartItems.length > 0" class="cart-footer">
      <!-- ... (Konten cart-footer tidak berubah) ... -->
      <div class="total">
        <span>Total Harga</span>
        <span>Rp {{ totalPrice.toLocaleString('id-ID') }}</span>
      </div>
      <button @click="checkout" class="checkout-btn">
        <vue-feather type="send" class="btn-icon"></vue-feather>
        Lanjut ke WhatsApp
      </button>
    </div>

    <!-- 6. Tambahkan struktur HTML untuk Modal Konfirmasi -->
    <transition name="modal-fade">
      <div v-if="showClearCartConfirm" class="confirm-overlay">
        <div class="confirm-box">
          <h4>Konfirmasi</h4>
          <p>Anda yakin ingin menghapus semua item dari keranjang?</p>
          <div class="confirm-actions">
            <button @click="cancelClearCart" class="btn-cancel">Batal</button>
            <button @click="confirmClearCart" class="btn-confirm">Ya, Hapus</button>
          </div>
        </div>
      </div>
    </transition>

  </div>
</template>

<style scoped>
/* ... (SEMUA STYLE SEBELUMNYA TETAP SAMA) ... */
/* src/components/ShoppingCart.vue */
.cart-panel {
  position: fixed;
  top: 0;
  right: 0;
  height: 100%;
  width: 90vw;
  max-width: 380px;
  background-color: white;
  box-shadow: -4px 0 15px rgba(0,0,0,0.1);
  z-index: 1000;
  display: flex;
  flex-direction: column;
  transform: translateX(100%);
  transition: transform 0.4s cubic-bezier(0.25, 0.46, 0.45, 0.94);
}
.cart-panel.is-open {
  transform: translateX(0);
}

.cart-header {
  padding: 1rem 1.5rem;
  border-bottom: 1px solid #eee;
  flex-shrink: 0;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.cart-title {
  font-size: 1.2rem;
  font-weight: 600;
  display: flex;
  align-items: center;
  gap: 10px;
  margin-right: auto;
}
.close-btn {
  background: none;
  border: none;
  cursor: pointer;
  padding: 5px;
  color: #888;
  transition: color 0.2s;
}
.close-btn:hover {
  color: #333;
}

.clear-cart-btn {
  background: none;
  border: none;
  color: #e63946;
  cursor: pointer;
  padding: 5px;
  margin-right: 10px;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: color 0.2s;
}
.clear-cart-btn:hover {
  color: #a11d27;
}

.cart-body {
  flex-grow: 1;
  overflow-y: auto;
  padding: 1rem;
}

.cart-footer {
  padding: 1.5rem;
  border-top: 1px solid #eee;
  background: #fdfdfd;
  flex-shrink: 0;
  padding-bottom: calc(1.5rem + env(safe-area-inset-bottom));
}

.cart-empty {
  text-align: center;
  padding: 3rem 1rem;
  height: 100%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  color: #888;
}
.empty-icon {
  margin-bottom: 1rem;
  color: #ccc;
}
.cart-empty p {
  font-weight: 600;
  font-size: 1.1rem;
  margin-bottom: 0.5rem;
  color: #555;
}
.cart-empty span {
  font-size: 0.9rem;
}

.cart-item {
  display: flex;
  gap: 1rem;
  margin-bottom: 1rem;
}
.item-image {
  width: 70px;
  height: 70px;
  object-fit: cover;
  border-radius: 8px;
  flex-shrink: 0;
}
.item-details {
  flex-grow: 1;
}
.item-name {
  font-weight: 600;
  margin-bottom: 0.25rem;
  line-height: 1.2;
}
.item-price {
  color: #666;
  font-size: 0.9rem;
}
.item-quantity {
  display: flex;
  align-items: center;
  margin-top: 0.5rem;
}
.quantity-btn {
  width: 28px;
  height: 28px;
  border: 1px solid #ddd;
  background: #f9f9f9;
  cursor: pointer;
  border-radius: 50%;
  transition: background-color 0.2s;
}
.quantity-btn:hover {
  background-color: #f0f0f0;
}
.quantity-btn:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}
.item-quantity span {
  padding: 0 1rem;
  font-weight: 600;
  font-size: 1.1rem;
}

.total {
  display: flex;
  justify-content: space-between;
  font-size: 1.1rem;
  font-weight: 700;
  margin-bottom: 1rem;
}
.checkout-btn {
  width: 100%;
  padding: 0.8rem;
  background-color: var(--primary-color);
  color: white;
  border: none;
  border-radius: 8px;
  font-size: 1rem;
  font-weight: 600;
  cursor: pointer;
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 8px;
  transition: background-color 0.3s;
}
.checkout-btn:hover {
  background-color: var(--secondary-color);
}
.btn-icon {
  width: 18px;
}

/* 7. Tambahkan CSS untuk Modal Konfirmasi */
.confirm-overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.6);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 20; /* Harus di atas konten keranjang */
}
.confirm-box {
  background: white;
  padding: 2rem;
  border-radius: 12px;
  text-align: center;
  width: 90%;
  max-width: 300px;
  box-shadow: 0 5px 15px rgba(0,0,0,0.3);
}
.confirm-box h4 {
  margin-top: 0;
  margin-bottom: 0.5rem;
}
.confirm-box p {
  margin-bottom: 1.5rem;
  color: #666;
}
.confirm-actions {
  display: flex;
  gap: 1rem;
}
.confirm-actions button {
  flex-grow: 1;
  padding: 0.75rem;
  border: none;
  border-radius: 8px;
  font-weight: 600;
  cursor: pointer;
  transition: transform 0.2s;
}
.confirm-actions button:hover {
  transform: scale(1.05);
}
.btn-cancel {
  background-color: #eee;
  color: #333;
}
.btn-confirm {
  background-color: #e63946;
  color: white;
}

/* Transisi untuk modal */
.modal-fade-enter-active, .modal-fade-leave-active {
  transition: opacity 0.3s ease;
}
.modal-fade-enter-from, .modal-fade-leave-to {
  opacity: 0;
}
</style>

