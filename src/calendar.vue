<template>
  <div class="calendar">
    <div class="date">
      {{ month.year }} - {{ month.month.toString().padStart(2, '0') }}
    </div>
    <div class="content">
      <div class="header">
        <div class="week-label" v-for="item in week">{{ item }}</div>
      </div>
      <div class="detail">
        <div class="week" :key="`${month.year}-${month.month}${index}`" v-for="(week, index) in month.weekList">
          <div @click="$emit('click', item.view)" :class="{
            'calendar-item': true,
            'not-current-month': !item.isCurrentMonth,
            'hide': !item.isCurrentMonth && hideNotCurrentMonthDate,
            'weekend': item.isWeekend,
            'today': item.isToday
          }" :key="item.view" v-for="item in week">
            {{ item.date.toString().padStart(2, '0') }}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import time from 'tiny-time-js'
import calendar from 'tiny-time-js/esm/plugins/calendar'
import type { Calendar } from 'tiny-time-js/esm/plugins/calendar'
import { computed, reactive, watch } from 'vue';
import { zh } from './constants'

const dfConfig = time.week

const props = defineProps<{
  weekCfg?: Record<string, number>,
  date?: {
    year: number,
    month: number
  }
  hideNotCurrentMonthDate?: boolean
}>()

const emit = defineEmits<{
  (event: 'click', date: string): void
  (event: 'date-change', date: Calendar): void
}>()

time.install(calendar)

const today = time()

const date = reactive<{ year: number, month: number }>({
  year: today.date.getFullYear(),
  month: today.date.getMonth(),
})

const week = computed(() => Object.entries(props.weekCfg || dfConfig).sort((a, b) => a[1] - b[1]).map(i => zh.week[i[0]]))

const month = computed(() => {
  time.week = props.weekCfg || dfConfig
  return time().calender(date?.year || today.date.getFullYear(), date?.month || today.date.getMonth())
})

defineExpose({
  nextMonth() {
    console.log(1)
    if (date.month + 1 > 12) {
      date.year += 1
      date.month = 1
    } else {
      date.month += 1
    }
  },
  preMonth() {
    if (date.month - 1 < 1) {
      date.year -= 1
      date.month = 12
    } else {
      date.month -= 1
    }
  },
  nextYear() {
    date.year += 1
  },
  preYear() {
    date.year -= 1
  },
  setDate(year: number, month: number){
    date.year = year 
    date.month = month
  }
})

watch(month, () => {
  emit('date-change', month.value)
})

</script>

<style lang="less">
.calendar {
  width: 100%;
  min-height: 400px;
  box-sizing: border-box;
  padding-top: 10px;
  box-shadow: 0px 12px 32px 4px rgba(0, 0, 0, .04), 0px 8px 20px rgba(0, 0, 0, .08);
  background-color: white;

  .date {
    text-align: center;
    min-height: 40px;
    line-height: 40px;
  }

  .content {
    width: 100%;
    box-sizing: border-box;

    .header {
      width: 100%;
      display: flex;
      flex-direction: row;
      margin: 20px 0;
      box-sizing: border-box;

      .week-label {
        width: calc(100% / 7);
        text-align: center;
      }
    }

    .detail {
      display: flex;
      flex-direction: column;
      box-sizing: border-box;

      .week {
        width: 100%;
        display: flex;
        box-sizing: border-box;
        flex-direction: row;
      }

      .calendar-item {
        width: calc(100% / 7);
        height: 60px;
        cursor: pointer;
        box-sizing: border-box;
        padding: 4px;
        background-color: aliceblue;
        color: mediumslateblue;
        margin: 1px;
      }

      .not-current-month {
        color: silver;
      }

      .today {
        color: orange;
        text-decoration: underline;
      }

      .hide {
        visibility: hidden;
      }

      .weekend {
        background-color: gainsboro;
      }
    }
  }
}
</style>
