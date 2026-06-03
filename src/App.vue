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
    <div class="sun-corner">
      <div class="sun-core"></div>
      <div class="sun-ray" v-for="i in 12" :key="i" :style="{ transform: `rotate(${i * 30}deg)` }"></div>
    </div>
    <h1 class="app-title">日历</h1>
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
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 30px;
  height: 30px;
  border-radius: 50%;
  background: radial-gradient(circle, #FFF9C4, #FFD700, #FFA000);
  box-shadow: 0 0 15px rgba(255, 215, 0, 0.6), 0 0 30px rgba(255, 165, 0, 0.3);
  z-index: 2;
}

.sun-ray {
  position: absolute;
  top: 0;
  left: 50%;
  transform-origin: 50% 30px;
  width: 3px;
  height: 12px;
  margin-left: -1.5px;
  background: linear-gradient(to top, rgba(255, 215, 0, 0.8), rgba(255, 215, 0, 0));
  border-radius: 2px;
  z-index: 1;
}

.switch-color-btn {
  padding: 10px 24px;
  margin-bottom: 20px;
  border: 1px solid rgba(255, 255, 255, 0.3);
  border-radius: 20px;
  background: rgba(255, 255, 255, 0.1);
  color: #ffffff;
  font-size: 14px;
  cursor: pointer;
  transition: all 0.3s ease;
  backdrop-filter: blur(4px);
}

.switch-color-btn:hover {
  background: rgba(255, 255, 255, 0.2);
  border-color: rgba(255, 255, 255, 0.5);
  transform: scale(1.05);
}
</style>