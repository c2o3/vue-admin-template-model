<template>
  <div class="tab-container">
    <el-tabs v-model="activeName" style="margin-top:15px;" type="border-card">
      <el-tab-pane v-for="item in tabMapOptions" :key="item.key" :label="item.label" :name="item.key">
        <keep-alive>
          <tab-pane-bottom v-if="activeName==item.key" :type="item.key" @create="showCreatedTimes" />
        </keep-alive>
      </el-tab-pane>
    </el-tabs>
  </div>
</template>

<script>
import TabPaneBottom from './components/TabPaneBottom'

export default {
  name: 'TabBottom',
  components: { TabPaneBottom },
  data() {
    return {
      tabMapOptions: [
        { label: '消费项目', key: 'checked' },
        { label: '购买产品', key: 'list' }
      ],
      activeName: 'checked',
      createdTimes: 0
    }
  },
  watch: {
    activeName(val) {
      this.$router.push(`${this.$route.path}?tab=${val}`)
    }
  },
  created() {
    // init the default selected tab
    const tab = this.$route.query.tab
    if (tab) {
      this.activeName = tab
    }
  },
  methods: {
    showCreatedTimes() {
      this.createdTimes = this.createdTimes + 1
    }
  }
}
</script>

  <style scoped>
    .tab-container {
      width: 90%;
      margin: 0px;
      padding-top: 0px;
    }
  </style>
