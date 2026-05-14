<script setup>
import JobList from '@/components/JobList.vue'
import { onMounted, ref } from 'vue'

const jobs = ref([])
const isLoading = ref(true)
const errorMessage = ref('')

onMounted(async () => {
  try {
    const response = await fetch('/data/jobs.json')

    if (!response.ok) {
      throw new Error('Failed to fetch jobs')
    }

    const data = await response.json()
    jobs.value = data
  } catch (error) {
    errorMessage.value = 'Failed to load jobs.'
  } finally {
    isLoading.value = false
  }
})
</script>

<template>
  <div>
    <JobList :jobs="jobs" />
  </div>
</template>

<style scoped></style>
