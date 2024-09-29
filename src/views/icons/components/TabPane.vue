<template>
  <div class="box-card">
    <el-table :data="paginatedList" border fit highlight-current-row style="width: 100%">
      <el-table-column
        v-loading="loading"
        align="center"
        label="ID"
        width="65"
        element-loading-text="请给我点时间！"
      >
        <template slot-scope="scope">
          <span>{{ scope.row.id }}</span>
        </template>
      </el-table-column>

      <el-table-column width="180px" align="center" label="流水单号">
        <template slot-scope="scope">
          <span>{{ scope.row.timestamp | parseTime('{y}-{m}-{d} {h}:{i}') }}</span>
        </template>
      </el-table-column>

      <el-table-column width="100px" label="会员主卡">
        <template slot-scope="{row}">
          <span>{{ row.title }}</span>
          <el-tag>{{ row.type }}</el-tag>
        </template>
      </el-table-column>

      <el-table-column width="110px" align="center" label="储值卡号">
        <template slot-scope="scope">
          <span>{{ scope.row.author }}</span>
        </template>
      </el-table-column>

      <el-table-column width="120px" label="客户姓名">
        <template slot-scope="scope">
          <span>{{ scope.row.importance }}</span>
        </template>
      </el-table-column>

      <el-table-column align="center" label="客户电话" width="95">
        <template slot-scope="scope">
          <span>{{ scope.row.pageviews }}</span>
        </template>
      </el-table-column>
    </el-table>

    <el-pagination
      :current-page="currentPage"
      :page-size="pageSize"
      :total="list.length"
      layout="total, prev, pager, next, jumper"
      @current-change="handleCurrentChange"
    />
  </div>
</template>

<script>
export default {
  filters: {
    statusFilter(status) {
      const statusMap = {
        published: 'success',
        draft: 'info',
        deleted: 'danger'
      }
      return statusMap[status]
    }
  },
  props: {
    type: {
      type: String,
      default: 'CN'
    }
  },
  data() {
    return {
      list: [
        { id: 1, timestamp: Date.now(), title: '主卡1', type: 'VIP', author: '12345', importance: 3, pageviews: '123456789' },
        { id: 2, timestamp: Date.now(), title: '主卡2', type: '普通', author: '67890', importance: 2, pageviews: '987654321' },
        { id: 3, timestamp: Date.now(), title: '主卡2', type: '普通', author: '67890', importance: 2, pageviews: '987654321' },
        { id: 4, timestamp: Date.now(), title: '主卡2', type: '普通', author: '67890', importance: 2, pageviews: '987654321' },
        { id: 5, timestamp: Date.now(), title: '主卡2', type: '普通', author: '67890', importance: 2, pageviews: '987654321' },
        { id: 6, timestamp: Date.now(), title: '主卡2', type: '普通', author: '67890', importance: 2, pageviews: '987654321' },
        { id: 7, timestamp: Date.now(), title: '主卡2', type: '普通', author: '67890', importance: 2, pageviews: '987654321' },
        { id: 8, timestamp: Date.now(), title: '主卡2', type: '普通', author: '67890', importance: 2, pageviews: '987654321' },
        { id: 9, timestamp: Date.now(), title: '主卡2', type: '普通', author: '67890', importance: 2, pageviews: '987654321' },
        { id: 10, timestamp: Date.now(), title: '主卡2', type: '普通', author: '67890', importance: 2, pageviews: '987654321' },
        { id: 11, timestamp: Date.now(), title: '主卡2', type: '普通', author: '67890', importance: 2, pageviews: '987654321' },
        { id: 12, timestamp: Date.now(), title: '主卡2', type: '普通', author: '67890', importance: 2, pageviews: '987654321' },
      ],
      listQuery: {
        page: 1,
        limit: 5,
        type: this.type,
        sort: '+id'
      },
      loading: false,
      currentPage: 1,
      pageSize: 4
    }
  },
  computed: {
    paginatedList() {
      const start = (this.currentPage - 1) * this.pageSize
      return this.list.slice(start, start + this.pageSize)
    }
  },
  created() {
    this.getList()
  },
  methods: {
    handleCurrentChange(page) {
      this.currentPage = page
    },
    getList() {
      this.loading = true
      this.$emit('create') // for test
      // fetchList(this.listQuery).then(response => {
      //   this.list = response.data.items;
      //   this.loading = false;
      // })
    }
  }
}
</script>
<style>
.box-card {
    max-height: 280px;
    overflow: scroll;
}
</style>
