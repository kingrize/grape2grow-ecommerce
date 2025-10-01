<!-- src/components/SearchBar.vue -->
<script>
import VueFeather from 'vue-feather';

export default {
  name: 'SearchBar',
  components: {
    VueFeather
  },
  props: {
    modelValue: String
  },
  emits: ['update:modelValue']
}
</script>

<template>
  <div class="search-wrapper">
    <vue-feather type="search" class="search-icon"></vue-feather>
    <input 
      type="text"
      class="search-input"
      placeholder="Cari bibit anggur berdasarkan nama, jenis, atau rasa..."
      :value="modelValue"
      @input="$emit('update:modelValue', $event.target.value)"
    >
  </div>
</template>

<style scoped>
/* src/components/SearchBar.vue */
.search-wrapper {
  position: relative;
  height: 100%; /* PERBAIKAN: Agar wrapper meregang */
}

.search-icon {
  position: absolute;
  top: 50%;
  left: 1rem;
  transform: translateY(-50%);
  color: #aaa;
  pointer-events: none;
}

.search-input {
  width: 100%;
  height: 100%; /* PERBAIKAN: Agar input meregang */
  box-sizing: border-box; /* Pastikan padding tidak menambah tinggi */
  padding: 1rem 1rem 1rem 3rem;
  font-size: 1rem;
  border: 1px solid #ddd;
  border-radius: 8px;
  transition: border-color 0.3s, box-shadow 0.3s;
}

.search-input:focus {
  outline: none;
  border-color: var(--primary-color);
  box-shadow: 0 0 0 3px rgba(76, 175, 80, 0.2);
}
</style>

