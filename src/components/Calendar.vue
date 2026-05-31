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
          'is-selected': isSelected(day),
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
  max-width: 400px;
  margin: 0 auto;
  background: var(--color-background-soft, #f8f8f8);
  border-radius: 12px;
  overflow: hidden;
  box-shadow: 0 2px 12px rgba(0, 0, 0, 0.08);
  user-select: none;
}

.calendar-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 16px 20px;
  background: #42b883;
  color: #fff;
}

.header-center {
  display: flex;
  align-items: center;
  gap: 12px;
}

.month-title {
  font-size: 18px;
  font-weight: 600;
  letter-spacing: 1px;
}

.nav-btn {
  width: 36px;
  height: 36px;
  border: none;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.2);
  color: #fff;
  font-size: 18px;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: background 0.2s;
}

.nav-btn:hover {
  background: rgba(255, 255, 255, 0.4);
}

.today-btn {
  padding: 2px 10px;
  border: 1px solid rgba(255, 255, 255, 0.6);
  border-radius: 14px;
  background: transparent;
  color: #fff;
  font-size: 13px;
  cursor: pointer;
  transition: background 0.2s;
}

.today-btn:hover {
  background: rgba(255, 255, 255, 0.2);
}

.calendar-weekdays {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
  background: var(--color-background, #fff);
  border-bottom: 1px solid var(--color-border, #eee);
}

.weekday {
  text-align: center;
  padding: 10px 0;
  font-size: 13px;
  font-weight: 600;
  color: var(--vt-c-text-light-2, #888);
}

.calendar-grid {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
  background: var(--color-background, #fff);
}

.day-cell {
  text-align: center;
  padding: 10px 0;
  font-size: 14px;
  cursor: pointer;
  border-radius: 8px;
  margin: 2px;
  transition: background 0.15s, color 0.15s;
  color: var(--color-text, #333);
}

.day-cell:hover {
  background: rgba(66, 184, 131, 0.15);
}

.day-cell.other-month {
  color: var(--color-border, #ccc);
  cursor: default;
}

.day-cell.other-month:hover {
  background: transparent;
}

.day-cell.is-today {
  background: #42b883;
  color: #fff;
  font-weight: 700;
  border-radius: 50%;
}

.day-cell.is-today:hover {
  background: #38a373;
}

.day-cell.is-selected {
  background: rgba(66, 184, 131, 0.25);
  color: #2c3e50;
  font-weight: 600;
}

.day-cell.is-selected.is-today {
  background: #42b883;
  color: #fff;
}
</style>