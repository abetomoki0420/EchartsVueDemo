<script setup lang="ts">
import { watch, nextTick, ref, computed } from 'vue'
import { init } from "echarts"

const chartElm = ref<HTMLElement>()

const year = ref("2022")
const month = ref("8")
const date = ref("1")
const weight = ref("80")

const xAxisData = ref<string[]>(["2022/7/28", "2022/7/29", "2022/7/30"])
const seriesData = ref<string[]>( ["77", "65", "54"])

const option = computed( () => ({
  xAxis: {
    type: 'category',
    data: xAxisData.value
  },
  yAxis: {
    type: 'value'
  },
  series: [
    {
      data: seriesData.value,
      type: 'line'
    }
  ]
}))

watch( option , () => {
  nextTick( () => {
    if(chartElm.value){
      const chart = init(chartElm.value)
      chart.setOption(option.value)
    }
  })
}, {
  immediate: true,
  deep: true
})

const input = () => {
  if(
     !year.value ||
     !month.value ||
     !date.value ||
     !weight.value
  ){
    return ;
  }

  xAxisData.value = [...xAxisData.value, `${year.value}/${month.value}/${date.value}`]
  seriesData.value = [...seriesData.value, weight.value]

  year.value = ""
  month.value = ""
  date.value = ""
  weight.value = ""
}

</script>

<template>
  <div>
    <div class="input">
      <label for="year">Year</label>
      <input id="year" v-model="year" type="number" min="2022" max="2030"/>
    </div>
    <div class="input">
      <label for="month">Month</label>
      <input id="month" v-model="month" type="number" min="1" max="12"/>
    </div>
    <div class="input">
      <label for="date">Date</label>
      <input id="date" v-model="date" type="number" min="1" max="31"/>
    </div>
    <div class="input">
      <label for="weight">Weight</label>
      <input id="weight" v-model="weight" type="number" min="40" max="100" />
    </div>
    <button @click="input">Input</button>
    <div class="chart" ref="chartElm"/>
  </div>
</template>

<style scoped>
.chart {
  height: 600px;
  width: 800px;
}

.input > label {
  display: inline-block;
  width: 60px;
}

.input > input {
  display: inline-block;
  width: 60px;
}
</style>