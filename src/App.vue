<script setup lang="ts">
import { ref } from 'vue';
import calendar from './calendar.vue'
import { weekCfg } from './constants';
import timejs from 'tiny-time-js';

function click(e: string) {
  alert(e)
}

const week = ref<typeof weekCfg | undefined>(undefined)
const calendarComp = ref<typeof calendar>()
const hide = ref(false)

// calendarComp
// function ()
// const calendarComp = 

</script>

<template>
  <div class="container">
    <div class="action">
      <div>
        每周第一天
      </div>
      <div>
        <button @click="week = undefined">周一</button>
        <button @click="week = weekCfg">周天</button>
      </div>
      <div>
        月操作
      </div>
      <div>
        <button @click="calendarComp?.preMonth()">previous</button>
        <button @click="calendarComp?.nextMonth()">next</button>
      </div>
      <div>
        年操作
      </div>
      <div>
        <button @click="calendarComp?.preYear()">previous</button>
        <button @click="calendarComp?.nextYear()">next</button>
      </div>
      <div>
        只展示本月
      </div>
      <div>
        <button @click="hide = true">是</button>
        <button @click="hide = false">否</button>
      </div>
      <div>
        其他
      </div>
      <div>
        <button @click="calendarComp?.setDate(timejs().date.getFullYear(), timejs().date.getMonth())">回到本月</button>
      </div>
    </div>
    <div class="content">
      <calendar ref="calendarComp" :hide-not-current-month-date="hide" @click="click" :week-cfg="week" />
    </div>
  </div>
</template>

<style scoped lang="less">
.container{
  display: flex;
  flex-direction: row;

  .action{
    flex-basis: 200px;
  }
  .content{
    flex: 1;
  }
}
</style>