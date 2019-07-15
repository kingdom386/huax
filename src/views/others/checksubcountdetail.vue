<template>
  <div>
    <el-form inline ref="countdetail" :model="countdetail" :rules="rules">
      <el-form-item label="交易会员代码:">
        <el-input placeholder="交易会员代码" v-model="countdetail.tradeMemCode"></el-input>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="init">查询</el-button>
      </el-form-item>
    </el-form>
    <el-table :data="tradedata" stripe border style="width: 100%">
      <el-table-column prop="order" label="序号"></el-table-column>
      <el-table-column prop="subAccount" label="子账号" width="160px"></el-table-column>
      <el-table-column prop="transCode" label="交易码"></el-table-column>
      <el-table-column prop="otherSubAccount" label="对方子账号"></el-table-column>
      <el-table-column prop="creMoney" label="发生额"></el-table-column>
      <el-table-column label="收付款标志">
        <template slot-scope="scope">
          <el-tag type="primary" v-if="scope.row.scope === '0'">付款</el-tag>
          <el-tag type="primary" v-else-if="scope.row.scope === '1'">收款</el-tag>
          <span v-else></span>
        </template>
      </el-table-column>
      <el-table-column prop="subAccountMoney" label="子账户余额"></el-table-column>
      <el-table-column prop="transCodeId" label="交易流水号"></el-table-column>
      <el-table-column prop="tradeTimes" label="交易时间"></el-table-column>
    </el-table>
  </div>
</template>

<script>
import { checktradedetail } from "@/api/table/subcounttable";
import { thirtyday } from '@/utils/validate'

export default {
  data() {
    return {
      merchantNo: "",
      tradedata: [],
      rules: {},
      countdetail: {
        tradeMemCode: "0000501763"
      }
    };
  },
  // watch: {
  //   tradeinfo(news, old) {
  //     this.init(news);
  //   }
  // },
  // mounted() {
  //   this.init(this.tradeinfo);
  // },
  methods: {
    init() {
      const _this = this;
      _this.tradedata = [];
      const date = new Date();
      const ls = {
        merchantNo: window.merchantNo,
        tradeMemCode: _this.countdetail.tradeMemCode,
        startTimes: this.$moment(thirtyday()).format("YYYYMMDDHHss"),
        endTimes: this.$moment(date).format("YYYYMMDDHHss")
      }
      checktradedetail(ls).then(res => {
        _this.tradedata = res.cycles;
      });
    }
  }
};
</script>
