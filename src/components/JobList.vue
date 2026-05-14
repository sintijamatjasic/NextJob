<script setup>
import JobCard from './JobCard.vue'

defineProps({
  jobs: Object,
})
</script>

<template>
  <div>
    <JobCard v-for="job in jobs" :key="job.id" :job="job" />
  </div>
</template>

<style scoped></style>
