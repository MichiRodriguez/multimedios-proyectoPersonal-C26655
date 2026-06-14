<template>
  <div class="overlay" @click.self="$emit('cerrar')">
    <div class="modal" role="dialog" :aria-label="estadio.nombre">
      <button class="btn-cerrar" @click="$emit('cerrar')">✕</button>

      <div class="modal-img-wrap">
        <img :src="estadio.imagen" :alt="estadio.nombre" class="modal-img" @error="onImgError" />
        <span v-if="estadio.esFinal" class="badge-final">🏆 SEDE DE LA FINAL</span>
      </div>

      <div class="modal-body">
      </div>
    </div>
  </div>
</template>

<script setup>
import { computed, ref, onUnmounted } from 'vue'

const props = defineProps({ estadio: Object })
defineEmits(['cerrar'])

const audioEl = ref(null)
const banderas = { USA: '🇺🇸', México: '🇲🇽', Canadá: '🇨🇦' }
const bandera = computed(() => banderas[props.estadio.pais] || '')

function onImgError(e) {
  e.target.src = 'https://placehold.co/800x400/1a2235/8896a8?text=Estadio'
}

onUnmounted(() => {
  if (audioEl.value) audioEl.value.pause()
})
</script>

<style scoped>
.overlay {
  position: fixed;
  inset: 0;
  background: rgba(0,0,0,0.75);
  z-index: 200;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 1rem;
  backdrop-filter: blur(4px);
}
.modal {
  background: var(--bg-card);
  border: 1px solid var(--border);
  border-radius: 20px;
  max-width: 700px;
  width: 100%;
  max-height: 90vh;
  overflow-y: auto;
  position: relative;
  box-shadow: 0 20px 60px rgba(0,0,0,0.6);
}
.btn-cerrar {
  position: absolute;
  top: 1rem;
  right: 1rem;
  z-index: 10;
  background: rgba(0,0,0,0.5);
  border: none;
  color: #fff;
  width: 2rem;
  height: 2rem;
  border-radius: 50%;
  cursor: pointer;
  font-size: 0.9rem;
}
.btn-cerrar:hover { background: var(--accent2); }

.modal-img-wrap {
  position: relative;
  height: 240px;
  border-radius: 20px 20px 0 0;
  overflow: hidden;
}
.modal-img { width: 100%; height: 100%; object-fit: cover; }
.badge-final {
  position: absolute;
  bottom: 0.8rem;
  left: 0.8rem;
  background: var(--accent);
  color: #000;
  font-weight: 700;
  font-size: 0.8rem;
  padding: 0.3rem 0.8rem;
  border-radius: 8px;
}

.modal-body { padding: 1.5rem; }
.modal-titulo { font-size: 1.4rem; font-weight: 700; margin-bottom: 0.1rem; }
.modal-local { font-size: 0.85rem; color: var(--accent); margin-bottom: 0.2rem; }
.modal-ciudad { color: var(--text-muted); font-size: 0.85rem; margin-bottom: 1rem; }
.modal-descripcion { line-height: 1.7; font-size: 0.92rem; margin-bottom: 1rem; }

.dato-curioso {
  background: var(--bg);
  border-left: 3px solid var(--accent);
  border-radius: 8px;
  padding: 0.85rem 1rem;
  margin-bottom: 1.25rem;
  font-size: 0.88rem;
  line-height: 1.6;
}
.dato-label {
  display: block;
  font-weight: 700;
  color: var(--accent);
  margin-bottom: 0.35rem;
  font-size: 0.8rem;
  text-transform: uppercase;
  letter-spacing: 0.5px;
}

.modal-datos {
  display: flex;
  gap: 0.75rem;
  flex-wrap: wrap;
  margin-bottom: 1.5rem;
}
.dato {
  flex: 1;
  min-width: 80px;
  background: var(--bg);
  border-radius: 10px;
  padding: 0.75rem 0.5rem;
  text-align: center;
  display: flex;
  flex-direction: column;
  gap: 0.2rem;
}
.dato-ico { font-size: 1.2rem; }
.dato-val { font-size: 0.95rem; font-weight: 700; color: var(--accent); }
.dato-val--sm { font-size: 0.75rem; }
.dato-lbl { font-size: 0.62rem; color: var(--text-muted); text-transform: uppercase; letter-spacing: 0.5px; }

.audio-section {
  background: var(--bg);
  border-radius: 12px;
  padding: 1rem;
}
.audio-label {
  font-weight: 600;
  color: var(--accent);
  font-size: 0.9rem;
  margin-bottom: 0.75rem;
}
.audio-player { width: 100%; border-radius: 8px; }
.proximamente { color: var(--text-muted); text-align: center; padding: 2rem; }
</style>
