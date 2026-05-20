<script setup>
import JobList from '@/components/JobList.vue'
import SearchBar from '@/components/SearchBar.vue'
import { computed, onMounted, ref } from 'vue'

const jobs = ref([])
const isLoading = ref(true)
const errorMessage = ref('')
const searchValue = ref('')

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

const filteredJobs = computed(() => {
  return jobs.value.filter((job) => {
    const matchesSearch =
      job.title.toLowerCase().includes(searchValue.value.toLowerCase()) ||
      job.company.toLowerCase().includes(searchValue.value.toLowerCase())
    return matchesSearch
  })
})
</script>

<template>
  <div class="page">
    <section class="hero">
      <div class="hero-content">
        <span class="top">Curated opportunities</span>

        <h1>Find your next role with clarity</h1>

        <p>
          Explore thoughtfully selected openings across product, design, engineering, and data — all
          in one clean, focused job search experience.
        </p>
      </div>

      <img class="hero-image" src="../../public/data/hero.png" alt="hero" />
    </section>

    <div class="controls">
      <div class="top-row">
        <SearchBar v-model="searchValue" />

        <button class="clear-btn">Clear filters</button>

        <button class="saved-btn"><i class="fa-regular fa-bookmark"></i> Saved Jobs</button>

        <button class="toggle-filters-btn">Show filters</button>
      </div>
    </div>
    <JobList :jobs="filteredJobs" />
  </div>
</template>

<style scoped>
.page {
  max-width: 1280px;
  margin: 0 auto;
  padding: 2rem 1.25rem 3rem;
}

.hero {
  display: grid;
  grid-template-columns: 1.1fr 0.9fr;
  gap: 1.5rem;
  align-items: stretch;
  margin-bottom: 1.75rem;
  padding: 1.5rem;
  border-radius: 20px;
  background: white;
  border: 1px solid #dbe3ee;
  box-shadow: 0 12px 30px rgba(15, 23, 42, 0.08);
}

.hero-content {
  display: flex;
  flex-direction: column;
  justify-content: center;
  gap: 1rem;
}

.top {
  font-size: 0.8rem;
  font-weight: 800;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  color: #4e6866;
}

.hero-content h1 {
  margin: 0;
  font-size: clamp(2.4rem, 5vw, 4.4rem);
  line-height: 0.95;
  letter-spacing: -0.04em;
}

.hero-content p {
  margin: 0;
  max-width: 58ch;
  line-height: 1.8;
  color: #4e6866;
}

.hero-image {
  width: 100%;
  height: 100%;
  min-height: 300px;
  object-fit: cover;
  border-radius: 16px;
}

.controls {
  display: flex;
  flex-direction: column;
  gap: 1rem;
  margin: 1.5rem 0 1rem;
}

.top-row {
  display: grid;
  grid-template-columns: minmax(0, 1fr) auto auto;
  gap: 1rem;
  align-items: center;
}

.clear-btn,
.saved-btn,
.toggle-filters-btn {
  border: 1px solid #dbe3ee;
  background: white;
  padding: 0.85rem 1rem;
  border-radius: 12px;
  font-size: 0.95rem;
  font-weight: 600;
  cursor: pointer;
  transition: 0.2s ease;
}

.clear-btn:hover,
.saved-btn:hover,
.toggle-filters-btn:hover {
  transform: translateY(-1px);
}

.saved-btn.active {
  background: #116a62;
  color: white;
  border-color: #116a62;
}

@media (max-width: 960px) {
  .hero {
    grid-template-columns: 1fr;
  }
}

@media (max-width: 860px) {
  .top-row {
    grid-template-columns: 1fr;
  }

  .clear-btn,
  .save-btn {
    width: 100%;
  }
}
</style>
