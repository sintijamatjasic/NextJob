<script setup>
defineProps({
  job: Object,
})
</script>

<template>
  <div class="job-card">
    <div class="job-top">
      <div class="company-block">
        <div class="company-logo">{{ job.logoText }}</div>

        <div class="job-heading">
          <h3 class="job-title">{{ job.title }}</h3>
          <p class="company-name">{{ job.company }}</p>
        </div>
      </div>

      <button class="save-btn">
        <i class="fa-regular fa-bookmark"></i>
      </button>
    </div>

    <div class="job-badges">
      <span class="badge">
        {{ job.category }}
      </span>

      <span v-if="job.remote" class="badge">Remote</span>
    </div>

    <div class="job-meta">
      <span class="meta-pill">
        {{ job.location }}
      </span>

      <span class="meta-pill">{{ job.level }}</span>

      <span class="meta-pill">{{ job.type }}</span>

      <span class="meta-pill">{{ job.postedDaysAgo }} day(s) ago</span>
    </div>

    <div class="job-bottom">
      <p class="salary">{{ job.salary }}€</p>

      <button class="details-btn">Details</button>
    </div>
  </div>
</template>

<style scoped>
.job-card {
  display: flex;
  flex-direction: column;
  gap: 1rem;
  padding: 1rem;
  border-radius: 16px;
  background: #ffffff;
  border: 1px solid #ced8e6;
  box-shadow: 0 10px 24px rgba(15, 23, 42, 0.06);
  transition: 0.2s ease;
}

.job-card:hover {
  transform: translateY(-3px);
  box-shadow: 0 14px 28px rgba(15, 23, 42, 0.08);
}

.job-top {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  gap: 1rem;
}

.company-block {
  display: flex;
  align-items: center;
  gap: 0.85rem;
  min-width: 0;
}

.company-logo {
  width: 48px;
  height: 48px;
  border-radius: 12px;
  display: grid;
  place-items: center;
  background: hsl(174, 100%, 94%);
  color: #116a62;
  font-weight: 700;
  flex-shrink: 0;
}

.job-heading {
  min-width: 0;
}

.job-title {
  margin: 0;
  font-size: 1rem;
  font-weight: 700;
  color: #101828;
}

.company-name {
  margin: 0.25rem 0 0;
  color: #667085;
}

.save-btn {
  width: 40px;
  height: 40px;
  border: 1px solid #dbe3ee;
  border-radius: 10px;
  background: white;
  color: #4e6866;
  cursor: pointer;
}

.save-btn.active {
  background: #116a62;
  color: white;
  border-color: #116a62;
}

.job-badges,
.job-meta {
  display: flex;
  flex-wrap: wrap;
  gap: 0.55rem;
}

.badge,
.meta-pill {
  display: inline-flex;
  align-items: center;
  padding: 0.4rem 0.7rem;
  border-radius: 999px;
  font-size: 0.85rem;
  font-weight: 600;
}

.badge {
  background-color: hsl(174, 100%, 96%);
  color: #116a62;
}

.meta-pill {
  background: rgb(247, 250, 255);
  color: #496967;
}

.job-bottom {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 1rem;
  margin-top: auto;
}

.salary {
  font-size: 1rem;
  font-weight: 700;
  color: #204d48;
}

.details-btn {
  border: none;
  background: #116a62;
  color: white;
  padding: 0.9rem 0.95rem;
  border-radius: 10px;
  font-size: 0.9rem;
  font-weight: 700;
  cursor: pointer;
}
</style>
