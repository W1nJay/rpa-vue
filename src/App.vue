<script setup>
import { ref } from 'vue'
import Calendar from './components/Calendar.vue'

const showCalendar = ref(true)
</script>

<template>
  <div class="app-container">
    <h1 class="app-title">日历</h1>
    <button class="toggle-btn" @click="showCalendar = !showCalendar">
      {{ showCalendar ? '隐藏日历' : '显示日历' }}
    </button>
    <Calendar v-show="showCalendar" />
  </div>
</template>

<style scoped>
.app-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding-top: 40px;
}

.app-title {
  font-size: 28px;
  font-weight: 700;
  color: var(--color-heading, #2c3e50);
  margin-bottom: 24px;
}

.toggle-btn {
  padding: 8px 16px;
  font-size: 14px;
  border: 1px solid var(--color-border);
  border-radius: 4px;
  background-color: var(--color-background-soft);
  color: var(--color-text);
  cursor: pointer;
  margin-bottom: 16px;
  transition: background-color 0.3s, border-color 0.3s;
}

.toggle-btn:hover {
  border-color: var(--color-border-hover);
  background-color: var(--color-background-mute);
}
</style>