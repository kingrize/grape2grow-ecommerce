<!-- src/components/ShoppingCart.vue -->
<script>
import VueFeather from 'vue-feather';

export default {
  name: 'ShoppingCart',
  components: {
    VueFeather
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
      this.$emit('close-cart');
    },
    // Method baru untuk mengirim sinyal hapus semua
    handleClearCart() {
      this.$emit('clear-cart');
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
      <!-- Tombol Hapus Semua -->
      <button v-if="cartItems.length > 0" @click="handleClearCart" class="clear-cart-btn">
        Hapus Semua
      </button>
      <button @click="handleCloseCart" class="close-btn" aria-label="Tutup Keranjang">
        <vue-feather type="x"></vue-feather>
      </button>
    </div>
    
    <div class="cart-body">
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
      <div class="total">
        <span>Total Harga</span>
        <span>Rp {{ totalPrice.toLocaleString('id-ID') }}</span>
      </div>
      <button @click="checkout" class="checkout-btn">
        <vue-feather type="send" class="btn-icon"></vue-feather>
        Lanjut ke WhatsApp
      </button>
    </div>
  </div>
</template>

<style scoped>
/* src/components/ShoppingCart.vue */
.cart-panel {
  position: fixed;
  top: 0;
  right: 0;
  height: 100%; /* Menggunakan 100% untuk kompatibilitas mobile yang lebih baik */
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
  margin-right: auto; /* Mendorong tombol lain ke kanan */
}
.close-btn {
  background: none;
  border: none;
  cursor: pointer;
  padding: 5px;
  color: #888;
}
.close-btn:hover {
  color: #333;
}

.clear-cart-btn {
  background: none;
  border: none;
  color: #e63946;
  font-size: 0.8rem;
  font-weight: 600;
  cursor: pointer;
  padding: 5px;
  margin-right: 10px;
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
  /* KUNCI PERBAIKAN: Menambahkan padding di bawah untuk memberi ruang dari navigasi browser */
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
</style>

