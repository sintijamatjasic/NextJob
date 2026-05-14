<script setup>
defineProps({
  job: Object,
})
</script>

<template>
  <div>
    <h3>{{ job.title }}</h3>
  </div>
</template>

<style scoped></style>
