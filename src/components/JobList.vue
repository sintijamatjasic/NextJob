<script setup>
import JobCard from './JobCard.vue'

defineProps({
  jobs: Object,
})
</script>

<template>
  <div class="job-list">
    <JobCard v-for="job in jobs" :key="job.id" :job="job" />
  </div>

  <div class="empty-state">
    <h3></h3>
    <p></p>
  </div>
</template>

<style scoped>
.job-list {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(320px, 1fr));
  gap: 1.3rem;
}
</style>
