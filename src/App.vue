<script setup>
import Calendar from './components/Calendar.vue'
</script>

<template>
  <div class="app-container">
    <h1 class="app-title">日历</h1>
    <Calendar />
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
</style>