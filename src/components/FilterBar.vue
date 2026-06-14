<template>
  <section class="filter-section">
    <div class="search-wrap">
      <span class="search-icon">🔍</span>
      <input
        class="search-input"
        type="text"
        placeholder="Buscar estadio o ciudad..."
        :value="busqueda"
        @input="$emit('update:busqueda', $event.target.value)"
      />
      <button v-if="busqueda" class="clear-btn" @click="$emit('update:busqueda', '')">✕</button>
    </div>

    <div class="filtros">
      <button
        v-for="p in paises"
        :key="p.valor"
        class="filtro-btn"
        :class="{ activo: paisActivo === p.valor }"
        @click="$emit('update:paisActivo', p.valor)"
      >
        {{ p.bandera }} {{ p.nombre }}
      </button>
    </div>

    <p class="resultado-count">
      {{ total }} estadio{{ total !== 1 ? 's' : '' }} encontrado{{ total !== 1 ? 's' : '' }}
    </p>
  </section>
</template>

<script setup>
defineProps({
  busqueda: String,
  paisActivo: String,
  total: Number
})
defineEmits(['update:busqueda', 'update:paisActivo'])

const paises = [
  { valor: 'todos', nombre: 'Todos',   bandera: '🌎' },
  { valor: 'USA',    nombre: 'USA',    bandera: '🇺🇸' },
  { valor: 'México', nombre: 'México', bandera: '🇲🇽' },
  { valor: 'Canadá', nombre: 'Canadá', bandera: '🇨🇦' }
]
</script>

<style scoped>
.filter-section {
  margin-bottom: 2rem;
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.search-wrap {
  position: relative;
  display: flex;
  align-items: center;
}
.search-icon {
  position: absolute;
  left: 1rem;
  pointer-events: none;
}
.search-input {
  width: 100%;
  padding: 0.85rem 3rem;
  background: var(--bg-card);
  border: 1px solid var(--border);
  border-radius: 12px;
  color: var(--text);
  font-size: 1rem;
  transition: border-color 0.2s;
}
.search-input::placeholder { color: var(--text-muted); }
.search-input:focus {
  outline: none;
  border-color: var(--accent);
}
.clear-btn {
  position: absolute;
  right: 1rem;
  background: none;
  border: none;
  color: var(--text-muted);
  cursor: pointer;
  font-size: 1rem;
}
.clear-btn:hover { color: var(--accent2); }

.filtros {
  display: flex;
  gap: 0.5rem;
  flex-wrap: wrap;
}
.filtro-btn {
  padding: 0.5rem 1.1rem;
  border-radius: 999px;
  border: 1px solid var(--border);
  background: var(--bg-card);
  color: var(--text-muted);
  cursor: pointer;
  font-size: 0.9rem;
  transition: all 0.2s;
}
.filtro-btn:hover { border-color: var(--accent); color: var(--accent); }
.filtro-btn.activo {
  background: var(--accent);
  border-color: var(--accent);
  color: #000;
  font-weight: 600;
}

.resultado-count {
  font-size: 0.82rem;
  color: var(--text-muted);
}
</style>
