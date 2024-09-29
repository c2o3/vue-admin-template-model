<template>
  <div class="app-container">
    <LineChart />
  </div>
</template>

<script>
import LineChart from './components/LineChart'
export default {
  name: 'SimpleTable',
  components: {
    LineChart
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
    width: 70%;
  }
  </style>
