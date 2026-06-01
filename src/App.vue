<script setup>
import Calendar from './components/Calendar.vue'
</script>

<template>
  <div class="app-container">
    <h1 class="app-title">日历</h1>
    <div class="rainbow-container">
      <div class="sun sun-left"></div>
      <div class="rainbow"></div>
      <div class="sun sun-right"></div>
    </div>
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

.rainbow-container {
  position: relative;
  width: 420px;
  height: 200px;
  margin-bottom: 24px;
}

.rainbow {
  position: absolute;
  bottom: 0;
  left: 50%;
  transform: translateX(-50%);
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
}

.sun {
  width: 50px;
  height: 50px;
  border-radius: 50%;
  background: radial-gradient(circle, #FFF9C4, #FFD700, #FFA000);
  position: absolute;
  box-shadow: 0 0 20px rgba(255, 215, 0, 0.5), 0 0 40px rgba(255, 165, 0, 0.3);
}

.sun::before {
  content: '';
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 72px;
  height: 72px;
  border-radius: 50%;
  background: rgba(255, 215, 0, 0.2);
}

.sun::after {
  content: '';
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 94px;
  height: 94px;
  border-radius: 50%;
  background: rgba(255, 215, 0, 0.1);
}

.sun-left {
  left: 15px;
  top: 15px;
}

.sun-right {
  right: 15px;
  top: 15px;
}
</style>