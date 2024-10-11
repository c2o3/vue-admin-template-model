<template>
  <div id="chart3" class="chart" style="height: 320px;width:380px" />
</template>

<script>
import * as echarts from 'echarts'
import resize from '../mixins/resize'

export default {
  name: 'Yeji',
  mixins: [resize],
  data() {
    return {
      chart: null
    }
  },
  mounted() {
    this.initChart()
  },
  methods: {
    initChart() {
      const myChart = echarts.init(document.getElementById('chart3'))

      myChart.setOption({
        title: {
          top: 20,
          text: '项目类别业绩',
          textStyle: {
            fontWeight: 'normal',
            fontSize: 16
          },
          left: '1%'
        },
        legend: {
          icon: 'circle',
          itemWidth: 10,
          itemHeight: 5,
          itemGap: 13,
          data: ['A客', 'B客', 'C客'],
          textStyle: {
            fontSize: 12,
            color: 'black'
          },
          bottom: 0
        },
        grid: {
          top: 60,
          left: '3%',
          right: '2%',
          bottom: '10%',
          containLabel: true
        },
        xAxis: {
          type: 'category',
          data: ['洗吹', '剪发', '烫发', '染发', '护理']
        },
        yAxis: {
          type: 'value'
        },
        series: [
          {
            name: 'A客',
            data: [5000, 70000, 60000, 40000, 1000],
            type: 'bar',
            stack: 'Ad'
          },
          {
            name: 'B客',
            data: [3000, 65000, 20000, 80000, 300],
            type: 'bar',
            stack: 'Ad'
          },
          {
            name: 'C客',
            data: [1000, 10000, 0, 100000, 100],
            type: 'bar',
            stack: 'Ad'
          }
        ]
      })
    }
  }
}
</script>
