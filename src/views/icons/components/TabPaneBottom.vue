<template>
  <div class="box-card">
    <el-button type="primary" @click="dialogVisible = true">添加消费项目</el-button>
    <el-table :data="list" border fit highlight-current-row style="width: 70%" :span-method="arraySpanMethod">
      <el-table-column align="center" label="项目" width="180">
        <template slot-scope="scope">
          <span>{{ scope.row.project }}</span>
        </template>
      </el-table-column>
      <el-table-column align="center" label="支付方式" width="120">
        <template slot-scope="{ row }">
          <div v-for="(payment, index) in row.payments" :key="index">
            <span :style="{ color: payment.color }">{{ payment.name }}</span>
          </div>
        </template>
      </el-table-column>
      <el-table-column align="center" label="原价" width="100">
        <template slot-scope="scope">
          <span>{{ scope.row.originalPrice }}</span>
        </template>
      </el-table-column>
      <el-table-column align="center" label="折扣" width="100">
        <template slot-scope="scope">
          <span>{{ scope.row.discount }}</span>
        </template>
      </el-table-column>
      <el-table-column align="center" label="数量" width="80">
        <template slot-scope="scope">
          <span>{{ scope.row.quantity }}</span>
        </template>
      </el-table-column>
      <el-table-column align="center" label="金额" width="100">
        <template slot-scope="scope">
          <span>{{ scope.row.amount }}</span>
        </template>
      </el-table-column>
      <el-table-column align="center" label="发型师" width="100">
        <template slot-scope="scope">
          <span>{{ scope.row.barber }}</span>
        </template>
      </el-table-column>
      <el-table-column align="center" label="是否指定" width="80">
        <template slot-scope="scope">
          <span>{{ scope.row.assignedBarber ? '是' : '否' }}</span>
        </template>
      </el-table-column>
      <el-table-column align="center" label="助理" width="100">
        <template slot-scope="scope">
          <span>{{ scope.row.consultant }}</span>
        </template>
      </el-table-column>
      <el-table-column align="center" label="是否指定" width="80">
        <template slot-scope="scope">
          <span>{{ scope.row.assignedConsultant ? '是' : '否' }}</span>
        </template>
      </el-table-column>
      <el-table-column align="center" label="业绩部门" width="100">
        <template slot-scope="scope">
          <span>{{ scope.row.performanceDept }}</span>
        </template>
      </el-table-column>
      <el-table-column align="center" label="操作" width="200">
        <template slot-scope="scope">
          <el-button type="danger" @click="removeRow(scope.row)">删除</el-button>
          <el-button type="primary" @click="editRow(scope.row)">编辑</el-button>
        </template>
      </el-table-column>
    </el-table>

    <el-dialog title="添加消费项目" :visible.sync="dialogVisible" width="30%">
      <el-form :model="newRow" label-position="left" label-width="70px">
        <el-row>
          <el-col :span="12">
            <el-form-item label="项目">
              <el-select v-model="newRow.project" placeholder="选择项目" @change="updatePrice">
                <el-option
                  v-for="item in projectSuggestions"
                  :key="item.name"
                  :label="item.name"
                  :value="item.name"
                />
              </el-select>
            </el-form-item>
          </el-col>
          <el-col :span="12">
            <el-form-item label="支付方式">
              <el-select v-model="newPayment.name" placeholder="选择支付方式">
                <el-option
                  v-for="payment in paymentSuggestions"
                  :key="payment.name"
                  :label="payment.name"
                  :value="payment.name"
                />
              </el-select>
            </el-form-item>
          </el-col>
        </el-row>

        <el-row>
          <el-col :span="12">
            <el-form-item label="原价">
              <el-input v-model="newRow.originalPrice" placeholder="请输入原价" style="width: 200px;" readonly />
            </el-form-item>
          </el-col>
          <el-col :span="12">
            <el-form-item label="折扣">
              <el-input v-model="newRow.discount" placeholder="请输入折扣" style="width: 200px;" />
            </el-form-item>
          </el-col>
        </el-row>

        <el-row>
          <el-col :span="12">
            <el-form-item label="数量">
              <el-input v-model="newRow.quantity" placeholder="请输入数量" style="width: 200px;" />
            </el-form-item>
          </el-col>
          <el-col :span="12">
            <el-form-item label="金额">
              <el-input v-model="newRow.amount" placeholder="请输入金额" style="width: 200px;" />
            </el-form-item>
          </el-col>
        </el-row>

        <el-row>
          <el-col :span="12">
            <el-form-item label="发型师">
              <el-select v-model="newRow.barber" placeholder="选择发型师">
                <el-option v-for="barber in barberSuggestions" :key="barber" :label="barber" :value="barber" />
              </el-select>
            </el-form-item>
          </el-col>
          <el-col :span="12">
            <el-form-item label="是否指定">
              <el-select v-model="newRow.assignedBarber" placeholder="是否指定">
                <el-option label="是" :value="true" />
                <el-option label="否" :value="false" />
              </el-select>
            </el-form-item>
          </el-col>
        </el-row>

        <el-row>
          <el-col :span="12">
            <el-form-item label="助理">
              <el-select v-model="newRow.consultant" placeholder="选择助理">
                <el-option v-for="consultant in consultantSuggestions" :key="consultant" :label="consultant" :value="consultant" />
              </el-select>
            </el-form-item>
          </el-col>
          <el-col :span="12">
            <el-form-item label="是否指定">
              <el-select v-model="newRow.assignedConsultant" placeholder="是否指定">
                <el-option label="是" :value="true" />
                <el-option label="否" :value="false" />
              </el-select>
            </el-form-item>
          </el-col>
        </el-row>
        <el-row>
          <el-col :span="12">
            <el-form-item label="业绩部门">
              <el-select v-model="newRow.performanceDept" placeholder="选择业绩部门">
                <el-option v-for="dept in deptSuggestions" :key="dept" :label="dept" :value="dept" />
              </el-select>
            </el-form-item>
          </el-col>
        </el-row>
      </el-form>
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="addRow">确 定</el-button>
      </span>
    </el-dialog>

    <el-dialog title="编辑消费项目" :visible.sync="editDialogVisible" width="30%">
      <el-form :model="currentRow" label-position="left" label-width="70px">
        <!-- 同样的表单结构，可以复用上面的代码 -->
        <el-row>
          <el-col :span="12">
            <el-form-item label="项目">
              <el-select v-model="currentRow.project" placeholder="选择项目" @change="updatePrice(currentRow.project)">
                <el-option
                  v-for="item in projectSuggestions"
                  :key="item.name"
                  :label="item.name"
                  :value="item.name"
                />
              </el-select>
            </el-form-item>
          </el-col>
          <el-col :span="12">
            <el-form-item label="支付方式">
              <el-select v-model="newPayment.name" placeholder="选择支付方式">
                <el-option
                  v-for="payment in paymentSuggestions"
                  :key="payment.name"
                  :label="payment.name"
                  :value="payment.name"
                />
              </el-select>
            </el-form-item>
          </el-col>
        </el-row>
        <el-row>
          <el-col :span="12">
            <el-form-item label="原价">
              <el-input v-model="currentRow.originalPrice" placeholder="请输入原价" style="width: 200px;" readonly />
            </el-form-item>
          </el-col>
          <el-col :span="12">
            <el-form-item label="折扣">
              <el-input v-model="currentRow.discount" placeholder="请输入折扣" style="width: 200px;" />
            </el-form-item>
          </el-col>
        </el-row>
        <el-row>
          <el-col :span="12">
            <el-form-item label="数量">
              <el-input v-model="currentRow.quantity" placeholder="请输入数量" style="width: 200px;" />
            </el-form-item>
          </el-col>
          <el-col :span="12">
            <el-form-item label="金额">
              <el-input v-model="currentRow.amount" placeholder="请输入金额" style="width: 200px;" />
            </el-form-item>
          </el-col>
        </el-row>
        <el-row>
          <el-col :span="12">
            <el-form-item label="发型师">
              <el-select v-model="currentRow.barber" placeholder="选择发型师">
                <el-option v-for="barber in barberSuggestions" :key="barber" :label="barber" :value="barber" />
              </el-select>
            </el-form-item>
          </el-col>
          <el-col :span="12">
            <el-form-item label="是否指定">
              <el-select v-model="currentRow.assignedBarber" placeholder="是否指定">
                <el-option label="是" :value="true" />
                <el-option label="否" :value="false" />
              </el-select>
            </el-form-item>
          </el-col>
        </el-row>
        <el-row>
          <el-col :span="12">
            <el-form-item label="助理">
              <el-select v-model="currentRow.consultant" placeholder="选择助理">
                <el-option v-for="consultant in consultantSuggestions" :key="consultant" :label="consultant" :value="consultant" />
              </el-select>
            </el-form-item>
          </el-col>
          <el-col :span="12">
            <el-form-item label="是否指定">
              <el-select v-model="currentRow.assignedConsultant" placeholder="是否指定">
                <el-option label="是" :value="true" />
                <el-option label="否" :value="false" />
              </el-select>
            </el-form-item>
          </el-col>
        </el-row>
        <el-row>
          <el-col :span="12">
            <el-form-item label="业绩部门">
              <el-select v-model="currentRow.performanceDept" placeholder="选择业绩部门">
                <el-option v-for="dept in deptSuggestions" :key="dept" :label="dept" :value="dept" />
              </el-select>
            </el-form-item>
          </el-col>
        </el-row>
      </el-form>
      <span slot="footer" class="dialog-footer">
        <el-button @click="editDialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="updateRow">确 定</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
export default {
  data() {
    return {
      list: [],
      dialogVisible: false,
      editDialogVisible: false,
      currentRow: {},
      newRow: {
        project: '',
        originalPrice: '',
        discount: '',
        quantity: '',
        amount: '',
        barber: '',
        assignedBarber: '',
        consultant: '',
        assignedConsultant: '',
        performanceDept: '',
        payments: [] // 添加 payments 数组
      },
      newPayment: { name: '', color: 'black', discountedPrice: '' },
      projectSuggestions: [
        { name: '项目A', price: 100 },
        { name: '项目B', price: 200 },
        { name: '项目C', price: 300 }
      ],
      paymentSuggestions: [{ name: '微信', color: 'red' }, { name: '支付宝', color: 'blue' }],
      barberSuggestions: ['发型师A', '发型师B'],
      consultantSuggestions: ['助理A', '助理B'],
      deptSuggestions: ['部门A', '部门B']
    }
  },
  watch: {
    'newRow.discount': 'calculateAmount',
    'newRow.quantity': 'calculateAmount'
  },
  methods: {
    updatePrice(selectedProject) {
      const project = this.projectSuggestions.find(item => item.name === selectedProject)
      if (project) {
        this.newRow.originalPrice = project.price
        this.newRow.quantity = 1 // 默认数量
        this.calculateAmount() // 计算金额
      }
    },
    calculateAmount() {
      const { originalPrice, quantity, discount } = this.newRow
      const discountedPrice = originalPrice * (1 - (discount / 100))
      this.newRow.amount = discountedPrice * quantity // 更新金额
    },
    addRow() {
      // 将支付方式添加到 newRow 中
      if (this.newPayment.name) {
        this.newRow.payments.push({ name: this.newPayment.name, color: 'black' }) // 你可以设置适当的颜色
      }
      this.list.push({ ...this.newRow })
      this.dialogVisible = false
      this.resetNewRow()
    },
    resetNewRow() {
      this.newRow = {
        project: '',
        originalPrice: '',
        discount: '',
        quantity: '',
        amount: '',
        barber: '',
        assignedBarber: '',
        consultant: '',
        performanceDept: '',
        payments: [] // 确保重置 payments
      }
      this.newPayment = { name: '', color: 'black', discountedPrice: '' }
    },
    removeRow(row) {
      const index = this.list.indexOf(row)
      if (index > -1) {
        this.list.splice(index, 1)
      }
    },
    editRow(row) {
      this.currentRow = { ...row }
      this.editDialogVisible = true
    },
    updateRow() {
      this.calculateAmount() // 确保更新金额
      const index = this.list.findIndex(item => item === this.currentRow)
      if (index > -1) {
        this.$set(this.list, index, { ...this.currentRow }) // 更新数据
      }
      this.editDialogVisible = false
    },
    arraySpanMethod({ row, column, rowIndex, columnIndex }) {
      return [1, 1] // 示例：合并行或列的逻辑
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
.dialog-footer {
  display: flex;
  justify-content: flex-end;
}
.bordered-cell {
  border: 1px solid #dcdfe6;
  padding: 5px;
}
</style>
