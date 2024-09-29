<template>
  <div class="app-container">
    <div class="filter-container">
      <span>选择门店</span>
      <el-select v-model="selectedStore" style="width: 140px; margin-left: 10px;" class="filter-item" @change="handleFilter">
        <el-option v-for="item in storeOptions" :key="item.key" :label="item.label" :value="item.key" />
      </el-select>
      <el-button class="filter-item" type="primary" icon="el-icon-edit" style="margin-left: 10px;" @click="handleAddRow">
        添加行
      </el-button>
      <el-button class="filter-item" type="warning" style="margin-left: 10px;" :disabled="!canUndo" @click="undo">撤销</el-button>
    </div>

    <el-table :data="list" border fit highlight-current-row style="width: 60%; margin-top: 15px;">
      <el-table-column label="编号" prop="id" align="center" width="80">
        <template slot-scope="{ row }"><span>{{ row.id }}</span></template>
      </el-table-column>
      <el-table-column label="名称" width="150px" align="center">
        <template slot-scope="{ row }">
          <el-input v-if="row.isEditing" v-model="row.name" @blur="updateData(row)" />
          <span v-else>{{ row.name }}</span>
        </template>
      </el-table-column>
      <el-table-column label="是否启用" width="120px" align="center">
        <template slot-scope="{ row }">
          <el-radio-group v-if="row.isEditing" v-model="row.enable" style="display: flex;flex-direction: column;align-items: flex-start;justify-content: center;" @change="updateData(row)">
            <el-radio :label="true">启用</el-radio>
            <el-radio :label="false">禁用</el-radio>
          </el-radio-group>
          <span v-else>{{ row.enable ? '启用' : '禁用' }}</span>
        </template>
      </el-table-column>
      <el-table-column label="说明" width="300px" align="center">
        <template slot-scope="{ row }">
          <el-input v-if="row.isEditing" v-model="row.remark" type="textarea" @blur="updateData(row)" />
          <span v-else>{{ row.remark }}</span>
        </template>
      </el-table-column>
      <el-table-column label="操作" align="center" width="150">
        <template slot-scope="{ row }">
          <el-button v-if="!row.isEditing" size="mini" type="primary" @click="startEdit(row)">编辑</el-button>
          <el-button v-if="row.isEditing" size="mini" type="primary" @click="finishEdit(row)">确定</el-button>
          <el-button size="mini" type="danger" @click="handleDelete(row)">删除</el-button>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
export default {
  name: 'SimpleTable',
  data() {
    return {
      storeOptions: [
        { label: '门店1', key: 'store1' },
        { label: '门店2', key: 'store2' }
      ],
      selectedStore: 'store1',
      storeData: {
        store1: [
          { id: 1, name: '面部护理', enable: true, remark: 'Remark 1', isEditing: false },
          { id: 2, name: '眼部护理', enable: false, remark: 'Remark 2', isEditing: false }
        ],
        store2: [
          { id: 1, name: '身体护理', enable: true, remark: 'Remark 1', isEditing: false },
          { id: 2, name: '头发护理', enable: false, remark: 'Remark 2', isEditing: false }
        ]
      },
      list: [],
      actionStack: [],
      canUndo: false
    }
  },
  mounted() {
    this.handleFilter()
  },
  methods: {
    handleFilter() {
      this.list = [...this.storeData[this.selectedStore]]
    },
    handleAddRow() {
      const newData = { id: this.list.length + 1, name: '', enable: true, remark: '', isEditing: true }
      this.storeData[this.selectedStore].push(newData)
      this.list.push(newData)
      this.actionStack.push({ type: 'add', data: newData }) // 记录添加操作
      this.canUndo = true // 启用撤销按钮
    },
    startEdit(row) {
      row.oldData = { ...row }; // 记录旧数据
      row.isEditing = true;
    },
    finishEdit(row) {
      row.isEditing = false
      this.updateData(row)
    },
    updateData(row) {
      const index = this.storeData[this.selectedStore].findIndex(item => item.id === row.id);
      if (index !== -1) {
        this.actionStack.push({ type: 'update', data: row.oldData }); // 记录旧值
        this.$set(this.storeData[this.selectedStore], index, { ...row });
        this.canUndo = true; // 启用撤销按钮
      }
    },
    handleDelete(row) {
      const index = this.storeData[this.selectedStore].findIndex(item => item.id === row.id)
      this.actionStack.push({ type: 'delete', data: { ...row, index }}) // 记录删除操作和索引
      this.storeData[this.selectedStore] = this.storeData[this.selectedStore].filter(item => item.id !== row.id)
      this.handleFilter()
      this.canUndo = true // 启用撤销按钮
    },
    undo() {
      const lastAction = this.actionStack.pop()
      if (!lastAction) return

      let index

      switch (lastAction.type) {
        case 'add':
          this.storeData[this.selectedStore] = this.storeData[this.selectedStore].filter(item => item.id !== lastAction.data.id)
          break
        case 'update':
          this.storeData[this.selectedStore].findIndex(item => item.id === lastAction.data.id)
          if (index !== -1) {
            this.$set(this.storeData[this.selectedStore], index, lastAction.data) // 恢复旧数据
          }
          break
        case 'delete':
          this.storeData[this.selectedStore].splice(lastAction.data.index, 0, lastAction.data) // 插入到原始位置
          break
      }
      this.handleFilter() // 刷新列表
      this.canUndo = this.actionStack.length > 0 // 更新撤销按钮状态
    }
  }
}
</script>

<style lang="scss" scoped>
.app-container {
  width: 70%;
}
</style>
