<template>
  <div class="box-card">
    <el-table :data="list" border fit highlight-current-row style="width: 100%" :span-method="arraySpanMethod">

      <!-- 项目列 -->
      <el-table-column align="center" label="项目" width="180">
        <template slot-scope="scope">
          <span>{{ scope.row.project }}</span>
        </template>
      </el-table-column>

      <!-- 支付方式列 -->
      <el-table-column align="center" label="支付方式" width="120">
        <template slot-scope="{ row }">
          <div v-for="(payment, index) in row.payments" :key="index" class="bordered-cell">
            <span :style="{ color: payment.color || 'black' }">{{ payment.name }}</span>
          </div>
        </template>
      </el-table-column>

      <!-- 原价列 -->
      <el-table-column align="center" label="原价" width="100">
        <template slot-scope="scope">
          <span>{{ scope.row.originalPrice }}</span>
        </template>
      </el-table-column>

      <!-- 折扣列 -->
      <el-table-column align="center" label="折扣" width="100">
        <template slot-scope="scope">
          <span>{{ scope.row.discount }}</span>
        </template>
      </el-table-column>

      <!-- 折后价列 -->
      <el-table-column align="center" label="折后价" width="100">
        <template slot-scope="scope">
          <div v-for="(payment, index) in row.payments" :key="index" class="bordered-cell">
            <span>{{ payment.discountedPrice }}</span>
          </div>
        </template>
      </el-table-column>

      <!-- 数量列 -->
      <el-table-column align="center" label="数量" width="100">
        <template slot-scope="scope">
          <span>{{ scope.row.quantity }}</span>
        </template>
      </el-table-column>

      <!-- 手工列 -->
      <el-table-column align="center" label="手工" width="120">
        <template slot-scope="scope">
          <span>{{ scope.row.manual }}</span>
        </template>
      </el-table-column>

      <!-- 消耗列 -->
      <el-table-column align="center" label="消耗" width="80">
        <template slot-scope="scope">
          <span>{{ scope.row.consumption }}</span>
        </template>
      </el-table-column>

      <!-- 指定列 -->
      <el-table-column align="center" label="指定" width="80">
        <template slot-scope="scope">
          <span>{{ scope.row.assigned }}</span>
        </template>
      </el-table-column>

      <!-- 顾问列 -->
      <el-table-column align="center" label="顾问" width="80">
        <template slot-scope="scope">
          <span>{{ scope.row.consultant }}</span>
        </template>
      </el-table-column>

      <!-- 业绩部门列 -->
      <el-table-column align="center" label="业绩部门" width="100">
        <template slot-scope="scope">
          <span>{{ scope.row.performanceDept }}</span>
        </template>
      </el-table-column>

      <!-- 支付储值卡号列 -->
      <el-table-column align="center" label="支付储值卡号" width="150">
        <template slot-scope="{ row }">
          <div v-for="(payment, index) in row.payments" :key="index" class="bordered-cell">
            <a :href="'/path-to/' + row.cardId">{{ row.cardId }}</a>
          </div>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
export default {
  data() {
    return {
      list: [
        {
          project: '13384-瑞士V眼部',
          payments: [
            { id: 1, name: '储值账户', color: 'red', discountedPrice: '1080' },
            { id: 2, name: '销售账户', color: 'red', discountedPrice: '300' },
            { id: 3, name: '红利账户', color: 'red', discountedPrice: '200' }
          ],
          originalPrice: '1580',
          discount: '1',
          quantity: '1',
          manual: '0011202-张',
          consumption: '2',
          assigned: '2',
          consultant: '2',
          performanceDept: '销售部',
          cardId: '234254'
        },
        {
          project: '11001-秘密花园',
          payments: [{ id: 1, name: '销售账户', color: 'red', discountedPrice: '100' }],
          originalPrice: '238',
          discount: '1',
          quantity: '1',
          manual: '0011205-杨',
          consumption: '2',
          assigned: '2',
          consultant: '2',
          performanceDept: '销售部',
          cardId: '888'
        }
      ]
    }
  },
  methods: {
    arraySpanMethod({ row, column, rowIndex, columnIndex }) {
      // 对于需要合并的列（项目、原价、折扣、数量、手工、消耗等）
      if (columnIndex < 10) {
        const rowLength = row.payments.length
        if (rowLength > 1) {
          if (rowIndex === 0) {
            return [rowLength, 1] // 合并多行
          } else {
            return [0, 0] // 隐藏被合并的单元格
          }
        }
      }
    }
  }
}
</script>

  <style scoped>
  .box-card {
    height: 500px;
    max-height: 600px;
    overflow-y: auto;
  }

  .bordered-cell {
    border-bottom: 1px solid #e0e0e0; /* 添加底部边框 */
    padding: 5px;
  }
  </style>
