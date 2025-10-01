<!-- src/components/FilterSort.vue -->
<script>
import VueFeather from 'vue-feather';

export default {
  name: 'FilterSort',
  components: {
    VueFeather
  },
  props: {
    modelValue: String
  },
  emits: ['update:modelValue'],
  data() {
    return {
      options: [
        { value: 'default', text: 'Urutkan Berdasarkan' },
        { value: 'price-asc', text: 'Harga: Termurah' },
        { value: 'price-desc', text: 'Harga: Termahal' },
        { value: 'name-asc', text: 'Nama: A-Z' },
        { value: 'name-desc', text: 'Nama: Z-A' },
      ]
    }
  }
}
</script>

<template>
  <div class="filter-sort-wrapper">
    <div class="select-container">
      <vue-feather type="filter" class="select-icon"></vue-feather>
      <select 
        :value="modelValue"
        @change="$emit('update:modelValue', $event.target.value)"
        class="sort-select"
      >
        <option 
          v-for="option in options" 
          :key="option.value" 
          :value="option.value"
          :disabled="option.value === 'default'"
        >
          {{ option.text }}
        </option>
      </select>
    </div>
  </div>
</template>

<style scoped>
/* src/components/FilterSort.vue */
.filter-sort-wrapper {
  display: flex;
  height: 100%; /* PERBAIKAN: Agar wrapper meregang */
}

.select-container {
  position: relative;
  width: 100%; /* Agar container mengisi wrapper */
  height: 100%;
}

.select-icon {
  position: absolute;
  top: 50%;
  left: 1rem;
  transform: translateY(-50%);
  color: #aaa;
  pointer-events: none;
}

.sort-select {
  -webkit-appearance: none;
  -moz-appearance: none;
  appearance: none;
  background-color: white;
  border: 1px solid #ddd;
  padding: 1rem 1rem 1rem 3rem;
  border-radius: 8px;
  font-size: 1rem;
  font-weight: 600;
  color: #555;
  cursor: pointer;
  background-image: url("data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' fill='none' viewBox='0 0 20 20'%3e%3cpath stroke='%236b7280' stroke-linecap='round' stroke-linejoin='round' stroke-width='1.5' d='M6 8l4 4 4-4'/%3e%3c/svg%3e");
  background-position: right 0.7rem center;
  background-repeat: no-repeat;
  background-size: 1.25em 1.25em;
  width: 100%; /* PERBAIKAN: Agar select mengisi container */
  height: 100%; /* PERBAIKAN: Agar select meregang */
  box-sizing: border-box; /* Pastikan padding tidak menambah tinggi */
}
.sort-select:focus {
  outline: none;
  border-color: var(--primary-color);
  box-shadow: 0 0 0 3px rgba(76, 175, 80, 0.2);
}
</style>

