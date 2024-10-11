<template>
  <div class="wrap">
    <div id="ciyun1" style="height: 320px; width: 380px;" />
    <div id="ciyun2" style="height: 320px; width: 380px;" />
    <div id="ciyun3" style="height: 320px; width: 380px;" />
  </div>
</template>

<script>
import * as echarts from 'echarts'
import 'echarts-wordcloud'

export default {
  name: 'CiyunMultiple',
  data() {
    return {
      charts: []
    }
  },
  mounted() {
    this.initChart('ciyun1', this.getChartData1(), 'A客消费项目')
    this.initChart('ciyun2', this.getChartData2(), 'B客消费项目')
    this.initChart('ciyun3', this.getChartData3(), 'C客消费项目')
  },
  beforeDestroy() {
    // 销毁图表实例，释放资源
    this.charts.forEach(chart => {
      if (chart) {
        chart.dispose()
      }
    })
  },
  methods: {
    initChart(elementId, chartData, title) {
      const myChart = echarts.init(document.getElementById(elementId))
      myChart.setOption({
        title: {
          top: 0,
          text: title,
          textStyle: {
            fontWeight: 'bold',
            fontSize: 16
          },
          left: '2%'
        },
        grid: {
          top: 60,
          left: '2%',
          right: '2%',
          bottom: '10%',
          containLabel: true
        },
        tooltip: {
          show: true,
          borderColor: '#fe9a8bb3',
          borderWidth: 1,
          padding: [10, 15, 10, 15],
          confine: true,
          backgroundColor: 'rgba(255, 255, 255, .9)',
          textStyle: {
            color: 'hotpink',
            lineHeight: 22
          },
          extraCssText: 'box-shadow: 0 4px 20px -4px rgba(199, 206, 215, .7);border-radius: 4px;'
        },
        series: [
          {
            type: 'wordCloud',
            shape: 'pentagon',
            left: 'center',
            top: 'center',
            width: '100%',
            height: '100%',
            sizeRange: [14, 50],
            rotationRange: [0, 0],
            rotationStep: 0,
            gridSize: 25,
            drawOutOfBound: false,
            layoutAnimation: true,
            textStyle: {
              fontFamily: 'PingFangSC-Semibold',
              fontWeight: 600,
              color: function() {
                const colors = ['#fe9a8bb3', '#fe9a8bb3', '#fe9a8b03', '#9E87FFb3', '#9E87FFb3', '#9E87FFb3', '#fe9a8bb3', '#fe9a8bb3', '#fe9a8bb3', '#73DDFF', '#58D5FF']
                return colors[parseInt(Math.random() * 10)]
              }
            },
            emphasis: {
              focus: 'none'
            },
            data: chartData
          }
        ]
      })
      this.charts.push(myChart)
    },
    getChartData1() {
      return [
        { value: '50', name: '剪发48', textStyle: { shadowBlur: 4, shadowColor: '#ECEFFF', shadowOffsetY: 14, color: '#73DDFF' }},
        { value: '30', name: '洗吹' },
        { value: '29', name: '剪发98' },
        { value: '28', name: '绿色护理A' },
        { value: '27', name: '小孩剪发' },
        { value: '26', name: '【HD】设计师剪发' },
        { value: '25', name: '剪发128' },
        { value: '24', name: '小分子' },
        { value: '23', name: '基础染发' },
        { value: '22', name: '理发200' },
        { value: '21', name: '染前隔离' },
        { value: '20', name: '盖白发A' },
        { value: '16', name: '服帖A' },
        { value: '15', name: '去角质' },
        { value: '14', name: '原萃精剪' },
        { value: '13', name: '盖白发B' },
        { value: '12', name: '基础烫发' },
        { value: '11', name: '蛋白胶精华A' },
        { value: '10', name: '补发A' },
        { value: '9', name: '刘海移位A' }
      ]
    },
    getChartData2() {
      return [
        { value: '50', name: '剪发48', textStyle: { shadowBlur: 4, shadowColor: '#ECEFFF', shadowOffsetY: 14, color: '#73DDFF' }},
        { value: '30', name: '洗吹' },
        { value: '29', name: '剪发98' },
        { value: '28', name: '绿色护理A' },
        { value: '27', name: '小孩剪发' },
        { value: '26', name: '【HD】设计师剪发' },
        { value: '25', name: '剪发128' },
        { value: '24', name: '小分子' },
        { value: '23', name: '基础染发' },
        { value: '22', name: '理发200' },
        { value: '21', name: '染前隔离' },
        { value: '20', name: '盖白发A' },
        { value: '16', name: '服帖A' },
        { value: '15', name: '去角质' },
        { value: '14', name: '原萃精剪' },
        { value: '13', name: '盖白发B' },
        { value: '12', name: '基础烫发' },
        { value: '11', name: '蛋白胶精华A' },
        { value: '10', name: '补发A' },
        { value: '9', name: '刘海移位A' }
      ]
    },
    getChartData3() {
      return [
        { value: '50', name: '剪发48', textStyle: { shadowBlur: 4, shadowColor: '#ECEFFF', shadowOffsetY: 14, color: '#73DDFF' }},
        { value: '30', name: '洗吹' },
        { value: '29', name: '剪发98' },
        { value: '28', name: '绿色护理A' },
        { value: '27', name: '小孩剪发' },
        { value: '26', name: '【HD】设计师剪发' },
        { value: '25', name: '剪发128' },
        { value: '24', name: '小分子' },
        { value: '23', name: '基础染发' },
        { value: '22', name: '理发200' },
        { value: '21', name: '染前隔离' },
        { value: '20', name: '盖白发A' },
        { value: '16', name: '服帖A' },
        { value: '15', name: '去角质' },
        { value: '14', name: '原萃精剪' },
        { value: '13', name: '盖白发B' },
        { value: '12', name: '基础烫发' },
        { value: '11', name: '蛋白胶精华A' },
        { value: '10', name: '补发A' },
        { value: '9', name: '刘海移位A' }
      ]
    }
  }
}
</script>

<style scoped>
.wrap{
  width: 100%;
  display: flex;
}
</style>
