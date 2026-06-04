<script setup>
defineProps({
  job: Object,
})
</script>

<template>
  <div class="modal-back" @click="$emit('close')">
    <div class="modal-card" @click.stop>
      <button class="modal-close" @click="$emit('close')">
        <i class="fa-solid fa-xmark"></i>
      </button>

      <div class="modal-content">
        <div class="modal-header">
          <div class="title">
            <h2 class="modal-title">{{ job.title }}</h2>
            <button
              class="save-btn"
              :class="{ active: job.favorite }"
              @click="$emit('toggle-saved', job.id)"
            >
              <i :class="job.favorite ? 'fa-solid fa-bookmark' : 'fa-regular fa-bookmark'"></i>
            </button>
          </div>

          <p class="modal-company">{{ job.company }}</p>

          <div class="modal-meta">
            <span>{{ job.category }}</span>

            <span>{{ job.location }}</span>

            <span>{{ job.level }}</span>

            <span>{{ job.type }}</span>

            <span>{{ job.salary }}€</span>

            <span>{{ job.postedDaysAgo }} day(s) ago</span>
          </div>
        </div>

        <div class="modal-grid">
          <section class="modal-section">
            <h4>Description</h4>
            <p class="modal-copy">
              {{ job.description }}
            </p>
          </section>

          <section class="modal-section">
            <h4>Requirements</h4>
            <ul class="modal-list">
              <li v-for="requirement in job.requirements" :key="requirement">{{ requirement }}</li>
            </ul>
          </section>

          <section class="modal-section">
            <h4>Responsibilities</h4>
            <ul class="modal-list">
              <li v-for="responsibility in job.responsibilities" :key="responsibility">
                {{ responsibility }}
              </li>
            </ul>
          </section>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.modal-back {
  position: fixed;
  inset: 0;
  background: rgba(15, 23, 42, 0.45);
  backdrop-filter: blur(4px);
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 1.25rem;
  z-index: 1000;
}

.modal-card {
  position: relative;
  width: min(900px, 100%);
  max-height: 90vh;
  overflow: auto;
  background: white;
  border-radius: 18px;
  border: 1px solid #dbe3ee;
  box-shadow: 0 30px 80px rgba(15, 23, 42, 0.18);
}

.modal-close {
  position: absolute;
  top: 1.5rem;
  right: 1rem;
  width: 42px;
  height: 42px;
  border: 1px solid #dbe3ee;
  border-radius: 10px;
  background: white;
  cursor: pointer;
  z-index: 2;
}

.modal-close:hover {
  color: rgb(255, 110, 110);
  border: 1px solid rgb(255, 110, 110);
  transition: 0.3s ease;
}

.modal-content {
  padding: 1.5rem;
  display: flex;
  flex-direction: column;
  gap: 1.4rem;
}

.modal-header {
  display: flex;
  flex-direction: column;
  gap: 0.8rem;
}

.modal-title {
  margin: 0;
  font-size: 1.8rem;
  line-height: 1.05;
}

.title {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 1rem;
}

.save-btn {
  width: 42px;
  height: 42px;
  border: 1px solid #dbe3ee;
  border-radius: 10px;
  background: white;
  color: #7b8794;
  cursor: pointer;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  transition: all 0.2s ease;
  margin-right: 3rem;
}

.save-btn:hover {
  transform: translateY(-1px);
  border-color: #f4c542;
}

.save-btn.active {
  background: #fff8db;
  color: #d4a017;
  border-color: #f4c542;
}

.save-btn.active i {
  color: #d4a017;
}

.save-btn:hover i {
  color: #d4a017;
}

.modal-company {
  margin: 0;
  color: #667085;
  font-size: 1rem;
}

.modal-meta {
  display: flex;
  flex-wrap: wrap;
  gap: 0.6rem;
}

.modal-meta span {
  padding: 0.42rem 0.7rem;
  border-radius: 999px;
  background: #f2f4f7;
  color: #475467;
  font-size: 0.9rem;
  font-weight: 600;
}

.modal-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1rem;
}

.modal-section {
  display: flex;
  flex-direction: column;
  gap: 0.8rem;
  padding: 1rem;
  border-radius: 14px;
  background: #f8fafc;
  border: 1px solid #e4e7ec;
}

.modal-section h4 {
  margin: 0;
  font-size: 1rem;
}

.modal-copy,
.modal-list {
  margin: 0;
  color: #475467;
  line-height: 1.7;
}

.modal-list {
  padding-left: 1.2rem;
}

@media (max-width: 700px) {
  .modal-grid {
    grid-template-columns: 1fr;
  }

  .modal-content {
    padding: 1rem;
  }

  .modal-section {
    padding: 0.9rem;
  }

  .modal-title,
  .modal-heading h2 {
    font-size: 1.6rem;
    line-height: 1.05;
  }

  .modal-close,
  .save-btn {
    width: 38px;
    height: 38px;
  }
}
</style>
