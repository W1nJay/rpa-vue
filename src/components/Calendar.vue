<script setup>
import { ref, computed } from 'vue'

const today = new Date()
const currentYear = ref(today.getFullYear())
const currentMonth = ref(today.getMonth())

const weekDays = ['日', '一', '二', '三', '四', '五', '六']

const monthNames = [
  '一月', '二月', '三月', '四月', '五月', '六月',
  '七月', '八月', '九月', '十月', '十一月', '十二月'
]

const selectedDate = ref(null)

const calendarDays = computed(() => {
  const year = currentYear.value
  const month = currentMonth.value
  const firstDay = new Date(year, month, 1).getDay()
  const daysInMonth = new Date(year, month + 1, 0).getDate()
  const daysInPrevMonth = new Date(year, month, 0).getDate()

  const days = []

  for (let i = firstDay - 1; i >= 0; i--) {
    days.push({ date: daysInPrevMonth - i, type: 'prev' })
  }

  for (let i = 1; i <= daysInMonth; i++) {
    days.push({ date: i, type: 'current' })
  }

  const remaining = 42 - days.length
  for (let i = 1; i <= remaining; i++) {
    days.push({ date: i, type: 'next' })
  }

  return days
})

const isToday = (day) => {
  if (day.type !== 'current') return false
  return (
    day.date === today.getDate() &&
    currentMonth.value === today.getMonth() &&
    currentYear.value === today.getFullYear()
  )
}

const isSelected = (day) => {
  if (day.type !== 'current' || !selectedDate.value) return false
  return (
    selectedDate.value.date === day.date &&
    selectedDate.value.month === currentMonth.value &&
    selectedDate.value.year === currentYear.value
  )
}

const selectDate = (day) => {
  if (day.type !== 'current') return
  selectedDate.value = {
    date: day.date,
    month: currentMonth.value,
    year: currentYear.value
  }
}

const prevMonth = () => {
  if (currentMonth.value === 0) {
    currentMonth.value = 11
    currentYear.value--
  } else {
    currentMonth.value--
  }
}

const nextMonth = () => {
  if (currentMonth.value === 11) {
    currentMonth.value = 0
    currentYear.value++
  } else {
    currentMonth.value++
  }
}

const goToday = () => {
  currentYear.value = today.getFullYear()
  currentMonth.value = today.getMonth()
  selectedDate.value = {
    date: today.getDate(),
    month: today.getMonth(),
    year: today.getFullYear()
  }
}
</script>

<template>
  <div class="calendar">
    <div class="calendar-header">
      <button class="nav-btn" @click="prevMonth">&lt;</button>
      <div class="header-center">
        <span class="month-title">{{ currentYear }}年 {{ monthNames[currentMonth] }}</span>
        <button class="today-btn" @click="goToday">今天</button>
        <button class="dummy-btn">按钮</button>
      </div>
      <button class="nav-btn" @click="nextMonth">&gt;</button>
    </div>

    <div class="calendar-weekdays">
      <div v-for="day in weekDays" :key="day" class="weekday">{{ day }}</div>
    </div>

    <div class="calendar-grid">
      <div
        v-for="(day, index) in calendarDays"
        :key="index"
        class="day-cell"
        :class="{
          'other-month': day.type !== 'current',
          'is-today': isToday(day),
          'is-selected': isSelected(day)
        }"
        @click="selectDate(day)"
      >
        {{ day.date }}
      </div>
    </div>
  </div>
</template>

<style scoped>
.calendar {
  width: 100%;
  background: rgba(255, 255, 255, 0.15);
  border-radius: 12px;
  padding: 20px;
  backdrop-filter: blur(10px);
}

.calendar-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 16px;
}

.header-center {
  display: flex;
  align-items: center;
  gap: 8px;
}

.month-title {
  font-size: 18px;
  font-weight: 600;
  color: #fff;
}

.nav-btn {
  background: rgba(255, 255, 255, 0.2);
  border: none;
  color: #fff;
  width: 36px;
  height: 36px;
  border-radius: 50%;
  cursor: pointer;
  font-size: 16px;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: background 0.3s;
}

.nav-btn:hover {
  background: rgba(255, 255, 255, 0.3);
}

.today-btn {
  background: rgba(255, 255, 255, 0.2);
  border: none;
  color: #fff;
  padding: 4px 12px;
  border-radius: 16px;
  cursor: pointer;
  font-size: 13px;
  transition: background 0.3s;
}

.today-btn:hover {
  background: rgba(255, 255, 255, 0.3);
}

.dummy-btn {
  background: rgba(255, 255, 255, 0.2);
  border: none;
  color: #fff;
  padding: 4px 12px;
  border-radius: 16px;
  font-size: 13px;
  cursor: pointer;
  transition: background 0.3s;
}

.dummy-btn:hover {
  background: rgba(255, 255, 255, 0.3);
}

.calendar-weekdays {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
  margin-bottom: 8px;
}

.weekday {
  text-align: center;
  font-size: 13px;
  color: rgba(255, 255, 255, 0.7);
  padding: 8px 0;
}

.calendar-grid {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
}

.day-cell {
  text-align: center;
  padding: 10px 0;
  border-radius: 50%;
  cursor: pointer;
  color: #fff;
  font-size: 14px;
  transition: background 0.2s;
}

.day-cell:hover {
  background: rgba(255, 255, 255, 0.15);
}

.day-cell.other-month {
  color: rgba(255, 255, 255, 0.35);
}

.day-cell.is-today {
  background: rgba(255, 255, 255, 0.25);
  font-weight: 700;
}

.day-cell.is-selected {
  background: #fff;
  color: #2196F3;
  font-weight: 700;
}
</style>