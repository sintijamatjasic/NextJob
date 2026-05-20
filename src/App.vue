<script setup>
import JobBoardPage from './pages/JobBoardPage.vue'
</script>

<template>
  <JobBoardPage />
</template>

<style>
* {
  box-sizing: border-box;
}

html {
  scroll-behavior: smooth;
}

body {
  margin: 0;
  font-family: 'Inter', sans-serif;
  background: #fafcff;
  color: #151f31;
}

button,
input,
select {
  font: inherit;
}

img {
  display: block;
  max-width: 100%;
}

#app {
  min-height: 100vh;
}
</style>
