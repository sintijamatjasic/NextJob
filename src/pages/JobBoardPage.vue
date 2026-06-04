<script setup>
import Filters from '@/components/Filters.vue'
import JobList from '@/components/JobList.vue'
import JobModal from '@/components/JobModal.vue'
import SearchBar from '@/components/SearchBar.vue'
import ActiveFilters from '@/components/ActiveFilters.vue'
import { computed, onMounted, ref } from 'vue'

const jobs = ref([])
const isLoading = ref(true)
const errorMessage = ref('')
const searchValue = ref('')
const showFilters = ref(false)
const categoryValue = ref('All')
const locationValue = ref('All')
const levelValue = ref('All')
const salaryValue = ref('All')
const sortValue = ref('Default')
const remoteOnly = ref(false)
const selectedJob = ref(null)
const savedOnly = ref(false)

const categories = computed(() => {
  return ['All', ...new Set(jobs.value.map((job) => job.category))]
})

const locations = computed(() => {
  return ['All', ...new Set(jobs.value.map((job) => job.location))]
})

const levels = computed(() => {
  return ['All', ...new Set(jobs.value.map((job) => job.level))]
})

const salaryRanges = ['All', 'Under 2000', '2000-3000', '3000-4000', '4000+']
const sortOptions = ['Default', 'Newest', 'Highest salary', 'A-Z']

onMounted(async () => {
  try {
    const response = await fetch('/data/jobs.json')

    if (!response.ok) {
      throw new Error('Failed to fetch jobs')
    }

    const data = await response.json()
    jobs.value = data

    loadSavedJobsFromStorage()
  } catch (error) {
    errorMessage.value = 'Failed to load jobs.'
  } finally {
    isLoading.value = false
  }
})

const filteredJobs = computed(() => {
  const filtered = jobs.value.filter((job) => {
    const matchesSearch =
      job.title.toLowerCase().includes(searchValue.value.toLowerCase()) ||
      job.company.toLowerCase().includes(searchValue.value.toLowerCase())

    const matchesCategory = categoryValue.value === 'All' || categoryValue.value === job.category
    const matchesLocation = locationValue.value === 'All' || locationValue.value === job.location
    const matchesLevel = levelValue.value === 'All' || levelValue.value === job.level
    const matchesSalaryRange =
      salaryValue.value === 'All' ||
      (salaryValue.value === 'Under 2000' && job.salary < 2000) ||
      (salaryValue.value === '2000-3000' && job.salary >= 2000 && job.salary < 3000) ||
      (salaryValue.value === '3000-4000' && job.salary >= 3000 && job.salary < 4000) ||
      (salaryValue.value === '4000+' && job.salary >= 4000)
    const matchesRemote = !remoteOnly.value || job.remote
    const matchesSaved = !savedOnly.value || job.favorite

    return (
      matchesSearch &&
      matchesCategory &&
      matchesLocation &&
      matchesLevel &&
      matchesSalaryRange &&
      matchesRemote &&
      matchesSaved
    )
  })

  const sorted = [...filtered]

  if (sortValue.value === 'Newest') {
    sorted.sort((a, b) => a.postedDaysAgo - b.postedDaysAgo)
  } else if (sortValue.value === 'Highest salary') {
    sorted.sort((a, b) => b.salary - a.salary)
  } else if (sortValue.value === 'A-Z') {
    sorted.sort((a, b) => a.title.localeCompare(b.title))
  }
  return sorted
})

function toggleFilterVisibility() {
  showFilters.value = !showFilters.value
}

function clearFilters() {
  searchValue.value = ''
  showFilters.value = false
  categoryValue.value = 'All'
  locationValue.value = 'All'
  levelValue.value = 'All'
  salaryValue.value = 'All'
  sortValue.value = 'Default'
  remoteOnly.value = false
  savedOnly.value = false
}

function openJobModal(job) {
  selectedJob.value = job
}

function closeJobModal() {
  selectedJob.value = null
}

function toggleSaved(jobId) {
  const job = jobs.value.find((item) => item.id === jobId)

  if (job) {
    job.favorite = !job.favorite
    saveSavedJobsToStorage()
  }
}

function saveSavedJobsToStorage() {
  const savedJobIds = jobs.value.filter((job) => job.favorite).map((job) => job.id)
  localStorage.setItem('savedJobs', JSON.stringify(savedJobIds))
}

function loadSavedJobsFromStorage() {
  const storedSavedJobs = localStorage.getItem('savedJobs')

  if (!storedSavedJobs) return

  const savedJobIds = JSON.parse(storedSavedJobs)

  jobs.value.forEach((job) => {
    job.favorite = savedJobIds.includes(job.id)
  })
}

const activeFilters = computed(() => {
  const filters = []

  if (categoryValue.value !== 'All') {
    filters.push({
      key: 'category',
      label: categoryValue.value,
    })
  }

  if (locationValue.value !== 'All') {
    filters.push({
      key: 'location',
      label: locationValue.value,
    })
  }

  if (levelValue.value !== 'All') {
    filters.push({
      key: 'level',
      label: levelValue.value,
    })
  }

  if (salaryValue.value !== 'All') {
    filters.push({
      key: 'salary',
      label: salaryValue.value,
    })
  }

  if (remoteOnly.value) {
    filters.push({
      key: 'remote',
      label: 'Remote only',
    })
  }

  if (sortValue.value !== 'Default') {
    filters.push({
      key: 'sort',
      label: sortValue.value,
    })
  }

  if (searchValue.value.trim() !== '') {
    filters.push({
      key: 'search',
      label: `Search: ${searchValue.value}`,
    })
  }

  return filters
})

function removeFilter(filterKey) {
  if (filterKey === 'category') {
    categoryValue.value = 'All'
  } else if (filterKey === 'location') {
    locationValue.value = 'All'
  } else if (filterKey === 'level') {
    levelValue.value = 'All'
  } else if (filterKey === 'salary') {
    salaryValue.value = 'All'
  } else if (filterKey === 'remote') {
    remoteOnly.value = false
  } else if (filterKey === 'saved') {
    savedOnly.value = false
  } else if (filterKey === 'sort') {
    sortValue.value = 'Default'
  } else if (filterKey === 'search') {
    searchValue.value = ''
  }
}
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

        <button class="saved-btn" :class="{ active: savedOnly }" @click="savedOnly = !savedOnly">
          <i :class="savedOnly ? 'fa-solid fa-bookmark' : 'fa-regular fa-bookmark'"></i> Saved Jobs
        </button>

        <button class="toggle-filters-btn" @click="toggleFilterVisibility">
          {{ showFilters ? 'Hide Filters' : 'Show Filters' }}
        </button>
      </div>
      <Filters
        v-if="showFilters"
        :categories="categories"
        :locations="locations"
        :levels="levels"
        :salaryRanges="salaryRanges"
        :sortOptions="sortOptions"
        v-model:selected-category="categoryValue"
        v-model:selected-location="locationValue"
        v-model:selected-level="levelValue"
        v-model:selected-salary-range="salaryValue"
        v-model:selected-sort="sortValue"
        v-model:remote-only="remoteOnly"
      />
      <ActiveFilters
        :filters="activeFilters"
        @remove-filter="removeFilter"
        @clear-all="clearFilters"
      />
    </div>

    <JobList :jobs="filteredJobs" @view-job="openJobModal" @toggle-saved="toggleSaved" />
    <transition name="modal-fade">
      <JobModal
        v-if="selectedJob"
        :job="selectedJob"
        @close="closeJobModal"
        @toggle-saved="toggleSaved"
    /></transition>
  </div>
</template>

<style scoped>
.page {
  max-width: 1280px;
  margin: 0 auto;
  padding: 2rem 1.25rem 3rem;
  overflow-x: hidden;
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

.saved-btn:hover,
.toggle-filters-btn:hover {
  transform: translateY(-1px);
}

.saved-btn.active {
  background: #116a62;
  color: white;
  border-color: #116a62;
}

.hero-content,
.controls,
.top-row {
  min-width: 0;
}

@media (max-width: 960px) {
  .hero {
    grid-template-columns: 1fr;
  }

  .hero-image {
    order: -1;
    min-height: 220px;
  }
}

@media (max-width: 860px) {
  .top-row {
    grid-template-columns: 1fr;
  }

  .saved-btn,
  .toggle-filters-btn {
    width: 100%;
  }
}

.modal-fade-enter-active,
.modal-fade-leave-active {
  transition: all 0.25s ease;
}
.modal-fade-enter-from,
.modal-fade-leave-to {
  opacity: 0;
  transform: scale(0.97);
}
</style>
