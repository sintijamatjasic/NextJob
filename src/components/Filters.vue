<script setup>
defineProps({
  categories: Array,
  locations: Array,
  levels: Array,
  salaryRanges: Array,
  sortOptions: Array,
})

const selectedCategory = defineModel('selectedCategory', { default: 'All' })
const selectedLocation = defineModel('selectedLocation', { default: 'All' })
const selectedLevel = defineModel('selectedLevel', { default: 'All' })
const selectedSalaryRange = defineModel('selectedSalaryRange', { default: 'All' })
const selectedSort = defineModel('selectedSort', { default: 'Default' })
const remoteOnly = defineModel('remoteOnly', { default: false })
</script>

<template>
  <div class="filters">
    <div class="filter-group">
      <p class="filter-label">Category</p>
      <div class="filter-buttons">
        <button
          v-for="category in categories"
          :key="category"
          class="filter-btn"
          :class="{ active: selectedCategory === category }"
          @click="selectedCategory = category"
        >
          {{ category }}
        </button>
      </div>
    </div>

    <div class="filter-group">
      <p class="filter-label">Location</p>
      <div class="filter-buttons">
        <button
          v-for="location in locations"
          :key="location"
          class="filter-btn"
          :class="{ active: selectedLocation === location }"
          @click="selectedLocation = location"
        >
          {{ location }}
        </button>
      </div>
    </div>

    <div class="filter-group">
      <p class="filter-label">Level</p>
      <div class="filter-buttons">
        <button
          v-for="level in levels"
          :key="level"
          class="filter-btn"
          :class="{ active: selectedLevel === level }"
          @click="selectedLevel = level"
        >
          {{ level }}
        </button>
      </div>
    </div>

    <div class="filter-group">
      <p class="filter-label">Salary (€)</p>
      <div class="filter-buttons">
        <button
          v-for="salaryRange in salaryRanges"
          :key="salaryRange"
          class="filter-btn"
          :class="{ active: selectedSalaryRange === salaryRange }"
          @click="selectedSalaryRange = salaryRange"
        >
          {{ salaryRange }}
        </button>
      </div>
    </div>

    <div class="filter-group">
      <p class="filter-label">Sort By</p>
      <div class="filter-buttons">
        <button
          v-for="sortOption in sortOptions"
          :key="sortOption"
          class="filter-btn"
          :class="{ active: selectedSort === sortOption }"
          @click="selectedSort = sortOption"
        >
          {{ sortOption }}
        </button>
      </div>
    </div>

    <div class="filter-group">
      <p class="filter-label">Remote</p>
      <div class="filter-buttons">
        <button
          class="filter-btn"
          :class="{ active: remoteOnly }"
          @click="remoteOnly = !remoteOnly"
        >
          Remote
        </button>
      </div>
    </div>
  </div>
</template>

<style scoped>
.filters {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.filter-group {
  display: flex;
  flex-direction: column;
  gap: 0.65rem;
  padding: 1rem;
  border-radius: 14px;
  background: white;
  border: 1px solid #dbe3ee;
}

.filter-label {
  margin: 0;
  font-size: 0.8rem;
  font-weight: 700;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  color: #667085;
}

.filter-buttons {
  display: flex;
  flex-wrap: wrap;
  gap: 0.6rem;
}

.filter-btn {
  border: 1px solid #dbe3ee;
  background: #f8fafc;
  color: #344054;
  padding: 0.7rem 0.9rem;
  border-radius: 10px;
  font-size: 0.9rem;
  font-weight: 600;
  cursor: pointer;
  transition: 0.2s ease;
}

.filter-btn:hover {
  transform: translateY(-1px);
}

.filter-btn.active {
  background: #116a62;
  color: white;
  border-color: #116a62;
}
</style>
