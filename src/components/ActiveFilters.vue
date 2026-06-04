<script setup>
defineProps({
  filters: Array,
})
</script>

<template>
  <div v-if="filters.length" class="active-filters">
    <div class="chips">
      <button
        v-for="filter in filters"
        :key="filter"
        @click="$emit('remove-filter', filter.key)"
        class="filter-chip"
      >
        <span>{{ filter.label }}</span>
        <i class="fa-solid fa-xmark"></i>
      </button>
    </div>

    <button class="clear-all-btn" @click="$emit('clear-all')">Clear all</button>
  </div>
</template>

<style scoped>
.active-filters {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  gap: 1rem;
  padding: 0.95rem 1rem;
  border-radius: 14px;
  background: #ffffff;
  border: 1px solid #dbe3ee;
}

.chips {
  display: flex;
  flex-wrap: wrap;
  gap: 0.65rem;
}

.filter-chip {
  display: inline-flex;
  align-items: center;
  gap: 0.5rem;
  border: 1px solid #dbe3ee;
  background: #eff6ff;
  color: #1d4ed8;
  padding: 0.55rem 0.8rem;
  border-radius: 999px;
  font-size: 0.88rem;
  font-weight: 600;
  cursor: pointer;
  transition: 0.2s ease;
}

.filter-chip:hover {
  transform: translateY(-1px);
  background: #dbeafe;
}

.clear-all-btn {
  border: none;
  background: transparent;
  color: #667085;
  font-size: 0.9rem;
  font-weight: 600;
  cursor: pointer;
  white-space: nowrap;
}

.clear-all-btn:hover {
  color: #162033;
}

@media (max-width: 760px) {
  .active-filters {
    flex-direction: column;
    align-items: stretch;
  }

  .clear-all-btn {
    align-self: flex-start;
  }
}
</style>
