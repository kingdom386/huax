<template>
  <div>
    <el-row>
      <el-form inline ref="flowing" :model="flowing">
        <el-col :span="6">
          <el-form-item label="查询方式:">
            <el-select v-model="flowing.queryMode" clearable @change="modechange">
              <el-option label="按照交易日期查询" value="1"></el-option>
              <el-option label="按照交易时间查询" value="2"></el-option>
            </el-select>
          </el-form-item>
        </el-col>
        <el-col :span="9" v-show="ishowing">
          <el-form-item label="日期:">
            <el-date-picker
              v-model="flowing.times"
              type="daterange"
              range-separator="至"
              start-placeholder="开始日期"
              end-placeholder="结束日期"
            ></el-date-picker>
          </el-form-item>
        </el-col>
        <el-col :span="6">
          <el-form-item label="交易类型:">
            <el-select v-model="flowing.appType" clearable>
              <el-option label="全部" value="0"></el-option>
              <el-option label="入金" value="1"></el-option>
              <el-option label="出金" value="2"></el-option>
              <el-option label="支付" value="3"></el-option>
              <el-option label="资金冻结" value="4"></el-option>
              <el-option label="资金解冻" value="5"></el-option>
              <el-option label="入金同步" value="6"></el-option>
              <el-option label="出金同步" value="7"></el-option>
              <el-option label="结息" value="8"></el-option>
              <el-option label="991转账入金" value="9"></el-option>
            </el-select>
          </el-form-item>
        </el-col>
        <el-col :span="2">
          <el-button type="primary" @click="queryflowing('flowing')">查询</el-button>
        </el-col>
      </el-form>
    </el-row>
    <el-table :data="flowingdata">
      <el-table-column label="子账号" prop="subAccount"></el-table-column>
      <el-table-column label="序号" prop="order"></el-table-column>
      <el-table-column label="支付单号" prop="payCode"></el-table-column>
      <el-table-column label="交易码" prop="transCode"></el-table-column>
      <el-table-column label="付款账号" prop="payAcc"></el-table-column>
      <el-table-column label="付款账户名称" prop="payAccName"></el-table-column>
      <el-table-column label="付款方开户行名称" prop="payOpenBank"></el-table-column>
      <el-table-column label="收款账号" prop="revAcc"></el-table-column>
      <el-table-column label="收款账户名称" prop="revAccName"></el-table-column>
      <el-table-column label="收款方开户行名称" prop="revOpenBank"></el-table-column>
      <el-table-column label="交易额" prop="totalAmt"></el-table-column>
      <el-table-column label="发生额" prop="creMoney"></el-table-column>
      <el-table-column label="交易类型" prop="appType"></el-table-column>
      <el-table-column label="币种" prop="currCode"></el-table-column>
      <el-table-column label="手续费" prop="feeAmt"></el-table-column>
      <el-table-column label="客户自付手续费" prop="custPayFee"></el-table-column>
      <el-table-column label="商户代付手续费" prop="merchantPayFee"></el-table-column>
      <el-table-column label="错误码" prop="msgId"></el-table-column>
      <el-table-column label="错误信息" prop="msg"></el-table-column>
      <el-table-column label="交易日期" prop="tranDate"></el-table-column>
      <el-table-column label="商户流水号" prop="merchantSeqId"></el-table-column>
      <el-table-column label="交易状态" prop="tranStat"></el-table-column>
      <el-table-column label="处理状态" prop="sysStat"></el-table-column>
      <el-table-column label="子账户余额" prop="subAccountMoney"></el-table-column>
      <el-table-column label="系统流水号" prop="transCodeId"></el-table-column>
      <el-table-column label="交易时间" prop="tradeTimes"></el-table-column>
      <el-table-column label="交易摘要" prop="busiSummary"></el-table-column>
      <el-table-column label="备注1" prop="remark1"></el-table-column>
      <el-table-column label="备注2" prop="remark2"></el-table-column>
      <el-table-column label="备注3" prop="remark3"></el-table-column>
      <el-table-column label="备注4" prop="remark4"></el-table-column>
      <el-table-column label="备注5" prop="remark5"></el-table-column>
    </el-table>
  </div>
</template>

<script>
export default {
  data() {
    return {
      flowingdata: [],
      ishowing: false,
      flowing: {
        queryMode: "",
        times: "",
        appType: "",
        startTimes: "",
        endTimes: ""
      }
    };
  },
  methods: {
    modechange() {
      this.flowing.queryMode ? this.ishowing = true : this.ishowing = false
    },
    queryflowing() {
      console.log(this.flowing);
      console.log(JSON.stringify(this.flowing));
    }
  }
};
</script>
