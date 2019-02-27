<template>
    <div ref="keyboardChart" class="echarts"/>
</template>

<script>
import echarts from 'echarts'
export default {
  name: 'LineEcharts',
  mounted () {
    this.initChart()
  },
  beforeUpdate () {
    this.initChart()
  },
  updated () {
    this.initChart()
  },
  methods: {
    initChart () {
      this.chart = echarts.init(this.$refs.keyboardChart)
      this.setOptions()
    },
    setOptions () {
      const xAxisData = []
      const data = []
      const data2 = []
      for (let i = 0; i < 50; i++) {
        xAxisData.push(i)
        data.push((Math.sin(i / 5) * (i / 5 - 10) + i / 6) * 5)
        data2.push((Math.sin(i / 5) * (i / 5 + 10) + i / 6) * 3)
      }

      this.chart.setOption({
        title: {
          text: 'ECharts 入门示例'
        },
        tooltip: {},
        legend: {
          data: ['销量']
        },
        xAxis: {
          data: ['衬衫', '羊毛衫', '雪纺衫', '裤子', '高跟鞋', '袜子']
        },
        yAxis: {},
        series: [{
          name: '销量',
          type: 'bar',
          data: [5, 20, 36, 10, 10, 20]
        }]
      })
    }
  }
}
</script>

<style lang="scss" scoped>
    .echarts {
        height: 100%;
        width: 100%;
        user-select: none;
    }
</style>
