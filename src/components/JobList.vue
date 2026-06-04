<script setup>
import JobCard from './JobCard.vue'

defineProps({
  jobs: Object,
})
</script>

<template>
  <div v-if="jobs.length" class="job-list">
    <JobCard
      v-for="job in jobs"
      :key="job.id"
      :job="job"
      @view-job="$emit('view-job', $event)"
      @toggle-saved="$emit('toggle-saved', $event)"
    />
  </div>

  <div v-else class="empty-state">
    <h3><i class="fa-regular fa-face-frown"></i> No roles match your filters</h3>
    <p>Try adjusting your search, clearing some filters, or broadening the role criteria.</p>
  </div>
</template>

<style scoped>
.job-list {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(320px, 1fr));
  gap: 1.3rem;
}

.empty-state {
  text-align: center;
  padding: 3rem 1.25rem;
  border-radius: 18px;
  border: 1px solid #dbe3ee;
  background: white;
}

.empty-state h3 {
  margin: 0 0 0.6rem;
  font-size: 1.4rem;
}

.empty-state p {
  margin: 0;
  color: #667085;
  line-height: 1.6;
}
</style>
