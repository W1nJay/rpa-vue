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
  border: 1px solid var(--color-border);
  border-radius: 8px;
  overflow: hidden;
  background: var(--color-background);
}

.calendar-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 16px;
  background: var(--color-background-soft);
}

.header-center {
  display: flex;
  align-items: center;
  gap: 12px;
}

.month-title {
  font-size: 18px;
  font-weight: 600;
}

.nav-btn, .today-btn {
  background: none;
  border: 1px solid var(--color-border);
  border-radius: 4px;
  padding: 4px 12px;
  cursor: pointer;
  color: var(--color-text);
}

.nav-btn:hover, .today-btn:hover {
  background: var(--color-background-mute);
}

.calendar-weekdays {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
  background: var(--color-background-mute);
  border-bottom: 1px solid var(--color-border);
}

.weekday {
  padding: 8px;
  text-align: center;
  font-weight: 600;
  font-size: 14px;
}

.calendar-grid {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
}

.day-cell {
  padding: 12px;
  text-align: center;
  cursor: pointer;
  border: 1px solid transparent;
}

.day-cell:hover {
  background: var(--color-background-mute);
}

.other-month {
  color: #ccc;
}

.is-today {
  font-weight: bold;
  color: #42b883;
}

.is-selected {
  background: #42b883;
  color: white;
  border-radius: 50%;
}
</style>