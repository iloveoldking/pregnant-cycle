<template>
  <div>以 {{ stime }} 作为周期计算开始时间</div>
  <div class="flex">
    <input type="text" v-model="syear" :disabled="!editable" maxlength="4" />
    <div class="split">年</div>
    <input type="text" v-model="smonth" :disabled="!editable" maxlength="2" />
    <div class="split">月</div>
    <input type="text" v-model="sday" :disabled="!editable" maxlength="2" />
  </div>

  <div class="time-split">到</div>

  <div class="flex no-margin">
    <input type="text" v-model="eyear" :disabled="!editable" maxlength="4" />
    <div class="split">年</div>
    <input type="text" v-model="emonth" :disabled="!editable" maxlength="2" />
    <div class="split">月</div>
    <input type="text" v-model="eday" :disabled="!editable" maxlength="2" />
  </div>

  <button class="btn" @click="editable = !editable">{{ !editable ? '设置日期' : '取消' }}</button>

  <div class="flex">
    <div>当前周期：{{ cycle.weeks }}w</div>
    <div v-if="cycle.days > 0">&nbsp;+ {{ cycle.days }}</div>
    <div>（ {{ cycle.allWeeks }}w ），</div>
    <div>已 {{ cycle.allDays }} 天。</div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';
import dayjs from 'dayjs';
import duration from 'dayjs/plugin/duration';
dayjs.extend(duration);

const editable = ref(false);
const syear = ref(2021);
const smonth = ref(11);
const sday = ref(30);
const eyear = ref(dayjs().year());
const emonth = ref(dayjs().month() + 1);
const eday = ref(dayjs().date());

const stime = computed(() => `${syear.value}-${smonth.value}-${sday.value} 00:00:00`);
const etime = computed(() => `${eyear.value}-${emonth.value}-${eday.value} 00:00:00`);

const cycle = computed(() => {
  const timeDuration = dayjs.duration(dayjs(etime.value).valueOf() - dayjs(stime.value).valueOf());
  const allDays = Math.ceil(timeDuration.asDays());
  const allWeeks = timeDuration.asWeeks().toFixed(1);
  let weeks = Math.floor(allWeeks);
  let days = allDays - weeks * 7;
  if (days >= 7) {
    days = 0;
    weeks += 1;
  }
  return { allDays, allWeeks, weeks, days };
});
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
#app {
  padding: 30px;
}
.flex {
  display: flex;
  align-items: center;
  margin-top: 30px;
}
input {
  width: 60px;
  border-radius: 4px;
  outline: none;
  border: #bbb 1px solid;
  padding: 2px 5px;
  height: 28px;
}
.no-margin {
  margin: 0;
}
.split {
  margin: 0 5px;
  line-height: 28px;
}
.time-split {
  margin: 10px 0;
}
.btn {
  width: 80px;
  border-radius: 4px;
  outline: none;
  border: none;
  background: #1890ff;
  color: #fff;
  padding: 0 5px;
  cursor: pointer;
  margin-top: 30px;
  line-height: 28px;
}
</style>
