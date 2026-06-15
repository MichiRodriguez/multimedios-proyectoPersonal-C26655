<template>
  <article class="card" @click="$emit('abrir', estadio)">
    <div class="card-img-wrap">
      <img
        :src="estadio.imagen"
        :alt="estadio.nombre"
        class="card-img"
        loading="lazy"
        @error="onImgError"
      />
    </div>

    <div class="card-body">
      <h2 class="card-nombre">{{ estadio.nombre }}</h2>
      <p class="card-local">{{ estadio.nombreLocal }}</p>
      <p class="card-ciudad"><MapPin :size="13" /> {{ estadio.ciudad }}</p>

      <div class="card-stats">
        <div class="stat">
          <span class="stat-val">{{ estadio.capacidad.toLocaleString() }}</span>
          <span class="stat-lbl">Capacidad</span>
        </div>
        <div class="stat">
          <span class="stat-val">{{ estadio.inaugurado }}</span>
          <span class="stat-lbl">Inaugurado</span>
        </div>
        <div class="stat">
          <span class="stat-val">{{ estadio.partidos }}</span>
          <span class="stat-lbl">Partidos</span>
        </div>
      </div>

      <div class="card-tags">
        <span class="categoria-tag">{{ estadio.categoria }}</span>
        <span v-if="estadio.audio" class="audio-tag"><Headphones :size="12" /> Audio</span>
      </div>
    </div>
  </article>
</template>

<script setup>
import { computed } from 'vue'
import { MapPin, Headphones } from '@lucide/vue'

const props = defineProps({ estadio: Object })
defineEmits(['abrir'])

const banderas = { USA: '🇺🇸', México: '🇲🇽', Canadá: '🇨🇦' }
const bandera = computed(() => banderas[props.estadio.pais] || '')

function onImgError(e) {
  e.target.src = 'https://placehold.co/600x340/1a2235/8896a8?text=Estadio'
}
</script>

<style scoped>
.card {
  background: var(--bg-card);
  border: 1px solid var(--border);
  border-radius: 16px;
  overflow: hidden;
  cursor: pointer;
  transition: transform 0.2s, box-shadow 0.2s, border-color 0.2s;
}
.card:hover {
  transform: translateY(-4px);
  box-shadow: var(--shadow);
  border-color: var(--accent);
}

.card-img-wrap {
  position: relative;
  height: 180px;
  overflow: hidden;
}
.card-img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.3s;
}
.card:hover .card-img { transform: scale(1.05); }

.badge-final {
  position: absolute;
  top: 0.6rem;
  left: 0.6rem;
  background: var(--accent);
  color: #000;
  font-size: 0.7rem;
  font-weight: 700;
  padding: 0.25rem 0.5rem;
  border-radius: 6px;
}
.badge-pais {
  position: absolute;
  top: 0.6rem;
  right: 0.6rem;
  background: rgba(0,0,0,0.6);
  color: #fff;
  font-size: 0.72rem;
  padding: 0.25rem 0.5rem;
  border-radius: 6px;
  backdrop-filter: blur(4px);
}

.card-body { padding: 1.1rem; }
.card-nombre {
  font-size: 1rem;
  font-weight: 700;
  margin-bottom: 0.1rem;
  color: var(--text);
}
.card-local {
  font-size: 0.78rem;
  color: var(--accent);
  margin-bottom: 0.25rem;
}
.card-ciudad {
  font-size: 0.82rem;
  color: var(--text-muted);
  margin-bottom: 0.9rem;
}

.card-stats {
  display: flex;
  gap: 0.5rem;
  margin-bottom: 0.8rem;
}
.stat {
  flex: 1;
  background: var(--bg);
  border-radius: 8px;
  padding: 0.4rem 0.3rem;
  text-align: center;
}
.stat-val {
  display: block;
  font-size: 0.85rem;
  font-weight: 700;
  color: var(--accent);
}
.stat-lbl {
  display: block;
  font-size: 0.62rem;
  color: var(--text-muted);
  text-transform: uppercase;
  letter-spacing: 0.5px;
}

.card-tags { display: flex; gap: 0.4rem; flex-wrap: wrap; }
.categoria-tag {
  font-size: 0.7rem;
  padding: 0.2rem 0.6rem;
  border-radius: 999px;
  background: var(--badge-bg);
  color: var(--text-muted);
}
.audio-tag {
  font-size: 0.7rem;
  padding: 0.2rem 0.6rem;
  border-radius: 999px;
  background: rgba(201,162,39,0.15);
  color: var(--accent);
  display: flex;
  align-items: center;
  gap: 0.25rem;
}
</style>
