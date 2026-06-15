<template>
  <header class="header">
    <div class="header-inner">
      <div class="logo">
        <img src="../assets/logo.png" alt="FIFA World Cup" class="logo-icon" />
        <div>
          <h1 class="logo-title">Estadios Mundial 2026</h1>
          <p class="logo-sub">USA · México · Canadá</p>
        </div>
      </div>
      <button class="toggle-btn" @click="$emit('toggleDark')" :title="darkMode ? 'Modo claro' : 'Modo oscuro'">
        <Sun v-if="darkMode" :size="20" />
        <Moon v-else :size="20" />
      </button>
    </div>
  </header>
</template>

<script setup>
import { Sun, Moon } from '@lucide/vue'
defineProps({ darkMode: Boolean })
defineEmits(['toggleDark'])
</script>

<style scoped>
.header {
  background: linear-gradient(135deg, #0a2342 0%, #1a3a6b 50%, #0a2342 100%);
  border-bottom: 3px solid var(--accent);
  position: sticky;
  top: 0;
  z-index: 100;
}
.header-inner {
  max-width: 1200px;
  margin: 0 auto;
  padding: 1rem 1.5rem;
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.logo {
  display: flex;
  align-items: center;
  gap: 0.75rem;
}
.logo-icon { height: 3rem; width: auto; }
.logo-title {
  font-size: 1.4rem;
  font-weight: 700;
  color: var(--accent);
}
.logo-sub {
  font-size: 0.75rem;
  color: #94a3b8;
  letter-spacing: 2px;
  text-transform: uppercase;
}
.toggle-btn {
  background: rgba(255,255,255,0.1);
  border: 1px solid rgba(255,255,255,0.2);
  border-radius: 8px;
  padding: 0.5rem 0.75rem;
  font-size: 1.1rem;
  cursor: pointer;
  transition: background 0.2s;
}
.toggle-btn:hover { background: rgba(255,255,255,0.2); }
</style>
