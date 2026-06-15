<template>
  <div id="app-root">
    <AppHeader :darkMode="darkMode" @toggleDark="toggleDark" />

    <main class="main-content">
      <div v-if="cargando" class="estado-msg"><Loader2 :size="20" class="spin" /> Cargando estadios...</div>
      <div v-else-if="error" class="estado-msg error"><AlertCircle :size="20" /> {{ error }}</div>

      <template v-else>
        <FilterBar
          v-model:busqueda="busqueda"
          v-model:paisActivo="paisActivo"
          :total="estadiosFiltrados.length"
        />

        <div v-if="estadiosFiltrados.length === 0" class="estado-msg">
          No se encontraron estadios con esos criterios.
        </div>

        <div v-else class="grid">
          <StadiumCard
            v-for="e in estadiosFiltrados"
            :key="e.id"
            :estadio="e"
            @abrir="abrirModal"
          />
        </div>
      </template>
    </main>

    <StadiumModal
      v-if="estadioSeleccionado"
      :estadio="estadioSeleccionado"
      @cerrar="estadioSeleccionado = null"
    />

    <AppFooter />
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue'
import { Loader2, AlertCircle } from '@lucide/vue'
import AppHeader from './components/AppHeader.vue'
import AppFooter from './components/AppFooter.vue'
import FilterBar from './components/FilterBar.vue'
import StadiumCard from './components/StadiumCard.vue'
import StadiumModal from './components/StadiumModal.vue'

const darkMode = ref(true)
const estadios = ref([])
const cargando = ref(true)
const error = ref(null)
const busqueda = ref('')
const paisActivo = ref('todos')
const estadioSeleccionado = ref(null)

function toggleDark() {
  darkMode.value = !darkMode.value
  document.body.classList.toggle('light', !darkMode.value)
}

onMounted(async () => {
  try {
    const res = await fetch(import.meta.env.BASE_URL + 'data/estadios.json')
    if (!res.ok) throw new Error('No se pudo cargar el archivo JSON')
    estadios.value = await res.json()
  } catch (e) {
    error.value = e.message
  } finally {
    cargando.value = false
  }
})

const estadiosFiltrados = computed(() => {
  const q = busqueda.value.toLowerCase().trim()
  return estadios.value.filter(e => {
    const coincidePais = paisActivo.value === 'todos' || e.pais === paisActivo.value
    const coincideBusqueda = !q || e.nombre.toLowerCase().includes(q) || e.ciudad.toLowerCase().includes(q)
    return coincidePais && coincideBusqueda
  })
})

function abrirModal(estadio) {
  estadioSeleccionado.value = estadio
}
</script>

<style scoped>
.main-content {
  max-width: 1200px;
  margin: 0 auto;
  padding: 2rem 1rem;
}
.grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
  gap: 1.5rem;
}
.estado-msg {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
  color: var(--text-muted);
  padding: 4rem 1rem;
  font-size: 1.1rem;
}
.estado-msg.error { color: var(--accent2); }
@keyframes spin { to { transform: rotate(360deg); } }
.spin { animation: spin 1s linear infinite; }
</style>
