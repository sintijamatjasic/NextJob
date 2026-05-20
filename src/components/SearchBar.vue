<script setup>
const model = defineModel({ default: '' })
</script>

<template>
  <div class="search-bar">
    <label for="search">
      <i class="fa-solid fa-magnifying-glass"></i>
    </label>
    <input v-model="model" type="text" id="search" placeholder="Search jobs or companies" />
  </div>
</template>

<style scoped>
.search-bar {
  width: 100%;
  position: relative;
}

.search-bar input {
  width: 100%;
  height: 56px;
  padding: 0.95rem 1rem 0.95rem 3rem;
  border-radius: 12px;
  border: 1px solid #dbe3ee;
  background: white;
  color: #162033;
  outline: none;
  transition: 0.2s ease;
}

.search-bar input::placeholder {
  color: #98a2b3;
}

.search-bar input:focus {
  border-color: #116a62;
  box-shadow: 0 0 0 4px rgba(87, 131, 127, 0.12);
}

.search-bar label {
  position: absolute;
  top: 50%;
  left: 1rem;
  transform: translateY(-50%);
  color: #116a62;
  pointer-events: none;
}
</style>
