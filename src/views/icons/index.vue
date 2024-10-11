<template>
  <div class="app-container">
    <el-row style="display: flex" class="top_container" :span="8">
      <el-button-group style="padding-left: 10px">
        <el-button
          icon="el-icon-circle-plus-outline"
          @click="onSubmit"
        >新增</el-button>
        <el-button icon="el-icon-circle-plus-outline">开单</el-button>
        <el-button icon="el-icon-takeaway-box">保存</el-button>
        <el-button icon="el-icon-edit">修改</el-button>
        <el-button icon="el-icon-warning">作废</el-button>
        <el-button icon="el-icon-error">删除</el-button>
        <el-button icon="el-icon-refresh-left">撤销</el-button>
        <el-button icon="el-icon-refresh">刷新</el-button>
        <el-button icon="el-icon-printer">打印</el-button>
        <el-button icon="el-icon-more">更多操作</el-button>
      </el-button-group>
      <div class="unknown">
        <el-button-group>
          <el-button icon="el-icon-d-arrow-left" />
          <el-button icon="el-icon-arrow-left" />
          <el-button icon="el-icon-arrow-right" />
          <el-button icon="el-icon-d-arrow-right" />
        </el-button-group>
      </div>
      <div class="top_form">
        <el-form :inline="true" :model="form" class="demo-form-inline">
          <el-form-item label="查找单号">
            <el-input
              v-model="form.orderNumber"
              placeholder="请输入单号"
              style="width: 200px"
            />
          </el-form-item>
          <el-form-item>
            <el-button icon="el-icon-search">查找</el-button>
          </el-form-item>
          <el-form-item>
            <el-button icon="el-icon-search">更多查找</el-button>
          </el-form-item>
        </el-form>
      </div>
    </el-row>
    <el-row class="middle_container">
      <el-col class="middle_left">
        <el-col>
          <el-form ref="middleForm" :inline="true" :model="middleForm" class="demo-form-inline">
            <div style="margin-bottom: -15px;">
              <el-form-item label="水单备注">
                <el-input
                  v-model="middleForm.note"
                  placeholder="请输入水单备注"
                  style="width: 487px"
                />
              </el-form-item>
              <el-form-item label="顾客来源">
                <el-input
                  v-model="middleForm.source"
                  placeholder="请输入顾客来源"
                  style="width: 200px"
                />
              </el-form-item>
              <el-form-item label="发票号码">
                <el-input
                  v-model="form.invoiceNumber"
                  placeholder="请输入发票号码"
                  style="width: 200px"
                />
              </el-form-item>
            </div>
            <el-col>
              <el-divider />
            </el-col>
            <el-form-item
              label="单据时间"
              prop="contractTime"
              :rules="[
                { required: true, message: '请输入单据时间', trigger: 'blur' },
                { type: 'date', message: '单据时间'}
              ]"
            >
              <el-input
                v-model="middleForm.contractTime"
                placeholder="请输入单据时间"
                style="width: 200px"
              />
            </el-form-item>
            <el-form-item label="顾客姓名">
              <el-input
                v-model="middleForm.name"
                placeholder="请输入顾客姓名"
                style="width: 200px"
              />
            </el-form-item>
            <el-form-item
              label="流水单号"
              prop="contractNumber"
              :rules="[
                { required: true, message: '请输入流水单号', trigger: 'blur' },
                { type: 'date', message: '流水单号'}
              ]"
            >
              <el-input
                v-model="middleForm.orderNumber"
                placeholder="请输入流水单号"
                style="width: 200px"
              />
            </el-form-item>
            <el-form-item label="顾客电话">
              <el-input
                v-model="middleForm.phoneNumber"
                placeholder="请输入顾客电话"
                style="width: 200px"
              />
            </el-form-item>
            <div style="width: 100%;display: flex;">
              <div class="left" style="width: 50%;display: flex;flex-direction: column;justify-content: center;align-items: center;">
                <el-form-item
                  label="会员主卡"
                  prop="vipMainNumber"
                  :rules="[
                    { required: true, message: '请输入会员主卡', trigger: 'blur' },
                    { type: 'date', message: '会员主卡'}
                  ]"
                >
                  <el-input
                    v-model="middleForm.vipMainNumber"
                    placeholder="请输入会员主卡"
                    style="width: 200px"
                  />
                </el-form-item>
                <el-form-item label="储值卡号">
                  <el-input
                    v-model="middleForm.cardNumber"
                    placeholder="请输入储值卡号"
                    style="width: 200px"
                  />
                </el-form-item>
              </div>
              <div class="right" style="width: 50%;padding: 0;">
                <p style="margin-top: 5px;">性别 <span>女</span><span style="margin-left: 15px;">{{ middleForm.source }}</span></p>
                <p style="margin-top: 50px;">客数 {{ 1 }}</p>
              </div>
            </div>
          </el-form>
        </el-col>
        <el-col>
          <p>会员信息</p>
          <p>会员归属门店：<span>管理总部</span></p>
          <p>卡类别：<span style="color: red;">疗程储值</span></p>
          <p>剩余积分：<span style="color: red;">{{ 0 }}</span></p>
          <div class="account">
            <el-table :data="tableData" style="width: 94%;" border max-height="150">
              <el-table-column prop="account" label="账户" width="180" />
              <el-table-column prop="preBalance" label="前次余额" width="180" />
              <el-table-column prop="nowBalance" label="可用余额" width="180" />
            </el-table>
          </div>
        </el-col>
      </el-col>
      <el-col><TabRight /></el-col>
    </el-row>
    <el-row class="bottom_container">
      <TabBottom />
    </el-row>
  </div>
</template>

<script>
import TabRight from './TabRight'
import TabBottom from './TabBottom'
export default {
  components: {
    TabRight,
    TabBottom
  },
  data() {
    return {
      form: {
        orderNumber: ''
      },
      middleForm: {
        note: '',
        source: '',
        invoiceNumber: '',
        name: '',
        contractNumber: '',
        phoneNumber: '',
        contractTime: '',
        vipMainNumber: '',
        cardNumber: ''
      },
      tableData: [{
        account: '3-储值账户',
        preBalance: '300',
        nowBalance: '4900'
      }, {
        account: 'D-红利账户',
        preBalance: '200',
        nowBalance: '0'
      }]
    }
  },
  methods: {
    onSubmit() {
      this.$message('submit!')
    },
    onCancel() {
      this.$message({
        message: 'cancel!',
        type: 'warning'
      })
    }
  }
}
</script>

<style scoped lang="scss">
.app-container {
  width: 100%;
  .top_container {
    .unknown {
      margin-left: 10px;
    }
    .top_form {
      margin-left: 10px;
    }
  }
  .middle_container {
    margin-top: 10px;
    margin-left: 10px;
    display: flex;
    .middle_left {
      display: flex;
      .el-divider--horizontal{
        width: 96%;
        margin: 5px 0 !important;
      }
    }
  }
}
</style>
