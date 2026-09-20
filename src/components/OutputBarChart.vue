<template>
  <div ref="chartRef" class="chart-box"></div>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue'
import * as echarts from 'echarts'

const chartRef = ref(null)
let chart = null

const days = ['09/14','09/15','09/16','09/17','09/18','09/19','09/20']
const values = [18200, 21600, 20500, 23800, 22450, 25100, 28560]

const initChart = () => {
  if (!chartRef.value) return
  chart = echarts.init(chartRef.value)
  chart.setOption({
    backgroundColor: 'transparent',
    grid: { left: 48, right: 14, top: 28, bottom: 30 },
    tooltip: {
      trigger: 'axis',
      backgroundColor: 'rgba(12,20,40,.96)',
      borderColor: '#26365e',
      textStyle: { color: '#e8eefc' },
      formatter: (params) => {
        const item = params[0]
        return `${item.axisValue}<br/>产量：${item.value.toLocaleString()} 米`
      }
    },
    xAxis: {
      type: 'category',
      data: days,
      axisLine: { lineStyle: { color: '#304264' } },
      axisLabel: { color: '#93a8d2' },
      axisTick: { show: false }
    },
    yAxis: {
      type: 'value',
      name: '米',
      nameTextStyle: { color: '#8ea4d2' },
      splitLine: { lineStyle: { color: 'rgba(111,132,178,.16)' } },
      axisLabel: { color: '#93a8d2' }
    },
    series: [
      {
        name: '七日产量',
        type: 'bar',
        data: values,
        barWidth: 22,
        itemStyle: {
          borderRadius: [8, 8, 0, 0],
          color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [
            { offset: 0, color: '#4cc9ff' },
            { offset: 1, color: '#2f6bff' }
          ])
        },
        emphasis: { itemStyle: { color: '#62d6ff' } }
      }
    ]
  })
}

const resize = () => chart && chart.resize()

onMounted(() => {
  initChart()
  window.addEventListener('resize', resize)
})

onBeforeUnmount(() => {
  window.removeEventListener('resize', resize)
  chart && chart.dispose()
})
</script>

<style scoped>
.chart-box{
  width:100%;
  height:300px;
}
</style>
