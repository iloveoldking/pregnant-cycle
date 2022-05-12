<template>
  <div class="flex no-margin">
    <div>截至时间：</div>
    <input type="text" v-model="eyear" maxlength="4" />
    <div class="split">年</div>
    <input type="text" v-model="emonth" maxlength="2" />
    <div class="split">月</div>
    <input type="text" v-model="eday" maxlength="2" />
  </div>

  <div class="title">临床孕周，以 {{ lstime }} 作为周期计算开始时间</div>
  <div class="flex">
    <input type="text" v-model="lsyear" maxlength="4" />
    <div class="split">年</div>
    <input type="text" v-model="lsmonth" maxlength="2" />
    <div class="split">月</div>
    <input type="text" v-model="lsday" maxlength="2" />
  </div>

  <div class="flex text-red">
    <div>临床孕周：{{ lcycle.weeks }}w</div>
    <div v-if="lcycle.days > 0">&nbsp;+ {{ lcycle.days }}</div>
    <div>（ {{ lcycle.allWeeks }}w ），</div>
    <div>已 {{ lcycle.allDays }} 天。</div>
  </div>

  <div class="title">超声孕周，以 {{ cstime }} 作为周期计算开始时间</div>
  <div class="flex">
    <input type="text" v-model="csyear" maxlength="4" />
    <div class="split">年</div>
    <input type="text" v-model="csmonth" maxlength="2" />
    <div class="split">月</div>
    <input type="text" v-model="csday" maxlength="2" />
  </div>

  <div class="flex text-red">
    <div>超声孕周：{{ ccycle.weeks }}w</div>
    <div v-if="ccycle.days > 0">&nbsp;+ {{ ccycle.days }}</div>
    <div>（ {{ ccycle.allWeeks }}w ），</div>
    <div>已 {{ ccycle.allDays }} 天。</div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';
import dayjs from 'dayjs';
import duration from 'dayjs/plugin/duration';
dayjs.extend(duration);

const eyear = ref(dayjs().year());
const emonth = ref(dayjs().month() + 1);
const eday = ref(dayjs().date());

const lsyear = ref(2021);
const lsmonth = ref(11);
const lsday = ref(24);

const csyear = ref(2021);
const csmonth = ref(11);
const csday = ref(30);

const lstime = computed(() => `${lsyear.value}-${lsmonth.value}-${lsday.value}`);
const cstime = computed(() => `${csyear.value}-${csmonth.value}-${csday.value}`);
const etime = computed(() => `${eyear.value}-${emonth.value}-${eday.value}`);

const lcycle = computed(() => {
  const timeDuration = dayjs.duration(dayjs(etime.value).valueOf() - dayjs(lstime.value).valueOf());
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

const ccycle = computed(() => {
  const timeDuration = dayjs.duration(dayjs(etime.value).valueOf() - dayjs(cstime.value).valueOf());
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
  padding: 20px;
}
.flex {
  display: flex;
  align-items: center;
  margin-top: 15px;
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
.title {
  margin-top: 50px;
}
.text-red {
  color: #f00;
}
</style>
