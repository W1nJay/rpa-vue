<script setup>
import Calendar from './components/Calendar.vue'
import { ref } from 'vue'

const colors = [
  { name: '黑色', value: '#000000' },
  { name: '深蓝', value: '#0a192f' },
  { name: '深绿', value: '#0d2818' },
  { name: '深红', value: '#2a0a0a' },
  { name: '深紫', value: '#1a0a2e' },
  { name: '靛青', value: '#0c2340' },
  { name: '深棕', value: '#1a0f0a' },
  { name: '墨色', value: '#1a1a2e' },
]

const colorIndex = ref(0)

const switchColor = () => {
  colorIndex.value = (colorIndex.value + 1) % colors.length
  document.documentElement.style.setProperty('--color-background', colors[colorIndex.value].value)
}
</script>

<template>
  <div class="app-container">
    <div class="moon-corner">
      <div class="moon-body"></div>
      <div class="moon-shadow"></div>
    </div>
    <div class="sun-corner">
      <div class="sun-core"></div>
      <div class="sun-ray" v-for="i in 12" :key="i" :style="{ transform: `rotate(${i * 30}deg)` }"></div>
    </div>
    <h1 class="app-title">日历</h1>
    <div class="center-moon">
      <div class="center-moon-body"></div>
      <div class="center-moon-crater crater-1"></div>
      <div class="center-moon-crater crater-2"></div>
      <div class="center-moon-crater crater-3"></div>
      <div class="center-moon-glow"></div>
    </div>
    <div class="rainbow-container">
      <div class="sun sun-left"></div>
      <div class="rainbow"></div>
      <div class="sun sun-right"></div>
    </div>
    <button class="switch-color-btn" @click="switchColor">
      🎨 切换背景色 ({{ colors[colorIndex].name }})
    </button>
    <Calendar />
  </div>
</template>

<style scoped>
.app-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding-top: 40px;
  position: relative;
}

.app-title {
  font-size: 28px;
  font-weight: 700;
  color: var(--color-heading, #ffffff);
  margin-bottom: 24px;
}

.center-moon {
  position: relative;
  width: 120px;
  height: 120px;
  margin-bottom: 24px;
}

.center-moon-body {
  position: absolute;
  top: 0;
  left: 0;
  width: 120px;
  height: 120px;
  border-radius: 50%;
  background: radial-gradient(circle at 40% 40%, #fdf6e3, #f5deb3, #e8d5a3);
  box-shadow:
    0 0 30px rgba(253, 246, 227, 0.4),
    0 0 60px rgba(253, 246, 227, 0.2),
    0 0 100px rgba(253, 246, 227, 0.1);
}

.center-moon-crater {
  position: absolute;
  border-radius: 50%;
  background: radial-gradient(circle, rgba(200, 180, 140, 0.6), rgba(180, 160, 120, 0.3));
}

.crater-1 {
  width: 22px;
  height: 22px;
  top: 25px;
  left: 30px;
}

.crater-2 {
  width: 16px;
  height: 16px;
  top: 55px;
  left: 65px;
}

.crater-3 {
  width: 12px;
  height: 12px;
  top: 70px;
  left: 35px;
}

.center-moon-glow {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 180px;
  height: 180px;
  border-radius: 50%;
  background: radial-gradient(circle, rgba(253, 246, 227, 0.15), transparent 70%);
  pointer-events: none;
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

.sun-corner {
  position: fixed;
  top: 20px;
  right: 20px;
  width: 60px;
  height: 60px;
  z-index: 1000;
}

.sun-core {
  width: 60px;
  height: 60px;
  border-radius: 50%;
  background: radial-gradient(circle, #FFF9C4, #FFD700, #FFA000);
  box-shadow: 0 0 30px rgba(255, 215, 0, 0.6), 0 0 60px rgba(255, 165, 0, 0.3);
}

.sun-ray {
  position: absolute;
  top: 50%;
  left: 50%;
  width: 2px;
  height: 20px;
  background: linear-gradient(to top, rgba(255, 215, 0, 0.6), transparent);
  transform-origin: center 0;
  margin-left: -1px;
  margin-top: -40px;
}

.moon-corner {
  position: fixed;
  top: 20px;
  left: 20px;
  width: 50px;
  height: 50px;
  z-index: 1000;
}

.moon-body {
  width: 50px;
  height: 50px;
  border-radius: 50%;
  background: radial-gradient(circle at 40% 40%, #fdf6e3, #f5deb3);
  box-shadow: 0 0 20px rgba(253, 246, 227, 0.4), 0 0 40px rgba(253, 246, 227, 0.2);
}

.moon-shadow {
  position: absolute;
  top: -5px;
  left: 12px;
  width: 45px;
  height: 45px;
  border-radius: 50%;
  background: var(--color-background, #000000);
  transition: background 0.5s;
}

.switch-color-btn {
  padding: 10px 20px;
  border: 1px solid rgba(255, 255, 255, 0.2);
  border-radius: 8px;
  background: rgba(255, 255, 255, 0.1);
  color: var(--color-text, #e0e0e0);
  font-size: 14px;
  cursor: pointer;
  margin-bottom: 24px;
  transition: all 0.3s;
}

.switch-color-btn:hover {
  background: rgba(255, 255, 255, 0.2);
  border-color: rgba(255, 255, 255, 0.4);
}
</style>