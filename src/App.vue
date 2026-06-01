<script setup>
import Calendar from './components/Calendar.vue'
</script>

<template>
  <div class="app-container">
    <h1 class="app-title">日历</h1>
    <div class="rainbow"></div>
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
  color: var(--color-heading, #ffffff);
  margin-bottom: 24px;
}

.rainbow {
  width: 300px;
  height: 150px;
  border-radius: 150px 150px 0 0;
  background: radial-gradient(circle at 50% 100%, 
    transparent 58px, 
    red 58px, red 68px, 
    orange 68px, orange 78px, 
    yellow 78px, yellow 88px, 
    green 88px, green 98px, 
    blue 98px, blue 108px, 
    indigo 108px, indigo 118px, 
    violet 118px, violet 128px, 
    transparent 128px
  );
  margin-bottom: 24px;
}
</style>