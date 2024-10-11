<template>
  <div class="app-container">
    <LineChart />
    <Counts />
    <Yeji />
    <div class="second-row">
      <Ciyun />
    </div>
  </div>

</template>

<script>
import LineChart from './components/LineChart'
import Counts from './components/counts'
import Yeji from './components/yeji'
import Ciyun from './components/ciyun'
export default {
  name: 'SimpleTable',
  components: {
    LineChart,
    Counts,
    Yeji,
    Ciyun
  },
  data() {
    return {}
  },
  mounted() {
  },
  methods: {}
}
</script>

<style lang="scss" scoped>
  .app-container {
    width: 80%;
    display: flex;
    flex-wrap: wrap;
    gap: 20px;
    .second-row {
      width: 100%;
    }
  }
</style>
