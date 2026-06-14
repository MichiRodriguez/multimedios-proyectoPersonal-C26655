<template>
  <div id="app-root">
    <AppHeader :darkMode="darkMode" @toggleDark="toggleDark" />
    <main class="main-content">
      <p class="placeholder">⚽ Cargando estadios...</p>
    </main>
    <AppFooter />
  </div>
</template>

<script setup>
import { ref } from 'vue'
import AppHeader from './components/AppHeader.vue'
import AppFooter from './components/AppFooter.vue'

const darkMode = ref(true)

function toggleDark() {
  darkMode.value = !darkMode.value
  document.body.classList.toggle('light', !darkMode.value)
}
</script>

<style scoped>
.main-content {
  max-width: 1200px;
  margin: 0 auto;
  padding: 2rem 1rem;
}
.placeholder {
  color: var(--text-muted);
  text-align: center;
  margin-top: 4rem;
  font-size: 1.2rem;
}
</style>
