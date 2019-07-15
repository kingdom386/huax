<template>
  <div>
    <el-form inline ref="historytrade" :model="historytrade" :rules="rules">
      <el-row>
        <el-col :span="7">
          <el-form-item prop="startDate" label="开始日期:">
            <el-date-picker
              value-format="yyyyMMdd"
              v-model="historytrade.startDate"
              type="date"
              clearable
              placeholder="开始日期"
            ></el-date-picker>
          </el-form-item>
        </el-col>
        <el-col :span="7">
          <el-form-item prop="endDate" label="结束日期:">
            <el-date-picker
              value-format="yyyyMMdd"
              v-model="historytrade.endDate"
              type="date"
              clearable
              placeholder="结束日期"
            ></el-date-picker>
          </el-form-item>
        </el-col>
        <el-col :span="7">
          <el-form-item label="金额下限:" prop="minAmt">
            <el-input placeholder="金额下限" clearable v-model="historytrade.minAmt" maxlength="17"></el-input>
          </el-form-item>
        </el-col>
        <el-col :span="7">
          <el-form-item label="金额上限:" prop="maxAmt">
            <el-input placeholder="金额上限" clearable v-model="historytrade.maxAmt" maxlength="17"></el-input>
          </el-form-item>
        </el-col>
        <el-col :span="7">
          <el-form-item label="交易币种:" prop="tradeCurrency">
            <el-select v-model="historytrade.tradeCurrency" filterable clearable placeholder="请选择">
              <el-option
                v-for="(item, index) in options"
                :key="index"
                :label="item.label"
                :value="item.value"
              ></el-option>
            </el-select>
          </el-form-item>
        </el-col>
        <el-col :span="7">
          <el-form-item label="账户类型:" prop="accountType">
            <el-select v-model="historytrade.accountType" filterable clearable placeholder="请选择">
              <el-option
                v-for="(item, index) in options1"
                :key="index"
                :label="item.label"
                :value="item.value"
              ></el-option>
            </el-select>
          </el-form-item>
        </el-col>
        <el-col :span="7">
          <el-form-item label="收付标志:" prop="payPayeeSign">
            <el-select v-model="historytrade.payPayeeSign" filterable clearable placeholder="请选择">
              <el-option
                v-for="(item, index) in options2"
                :key="index"
                :label="item.label"
                :value="item.value"
              ></el-option>
            </el-select>
          </el-form-item>
        </el-col>
        <el-col :span="2">
          <el-button type="primary" @click="queryflowings('historytrade')">查询</el-button>
        </el-col>
      </el-row>
    </el-form>
    <el-table :data="historydata">
      <el-table-column label="序号" prop="order"></el-table-column>
      <el-table-column width="150px" label="子账号" prop="subAccount"></el-table-column>
      <el-table-column width="130px" label="支付单号" prop="payCode"></el-table-column>
      <el-table-column label="交易码" prop="transCode"></el-table-column>
      <el-table-column width="140px" label="付款账号" prop="payAcc"></el-table-column>
      <el-table-column label="付款账户名称" prop="payAccName"></el-table-column>
      <el-table-column label="付款方开户行名称" prop="payOpenBank"></el-table-column>
      <el-table-column label="收款账号" prop="revAcc"></el-table-column>
      <el-table-column label="收款账户名称" prop="revAccName"></el-table-column>
      <el-table-column label="收款方开户行名称" prop="revOpenBank"></el-table-column>
      <el-table-column label="交易额" prop="totalAmt"></el-table-column>
      <el-table-column label="发生额" prop="creMoney"></el-table-column>
      <el-table-column label="交易类型">
        <template slot-scope="scope">
          <el-tag type="primary" v-if="scope.row.appType === '1'">入金</el-tag>
          <el-tag type="primary" v-else-if="scope.row.appType === '2'">出金</el-tag>
          <el-tag type="primary" v-else-if="scope.row.appType === '3'">支付</el-tag>
          <el-tag type="primary" v-else-if="scope.row.appType === '4'">资金冻结</el-tag>
          <el-tag type="primary" v-else-if="scope.row.appType === '5'">资金解冻</el-tag>
          <el-tag type="primary" v-else-if="scope.row.appType === '6'">入金同步</el-tag>
          <el-tag type="primary" v-else-if="scope.row.appType === '7'">出金同步</el-tag>
          <el-tag type="primary" v-else-if="scope.row.appType === '8'">结息</el-tag>
          <el-tag type="primary" v-else-if="scope.row.appType === '9'">991转账入金</el-tag>
          <span v-else></span>
        </template>
      </el-table-column>
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
import { historytrade } from "@/api/table/subcounttable";
export default {
  data() {
    const checkminAmt = (rule, value, callback) => {
      const reg = /^([1-9][\d+]{0,200}|0)(\.[\d]{1,2})?$/;
      if (reg.test(value)) {
        callback();
      } else {
        callback(new Error("输入的金额有误！"));
      }
    };
    return {
      historydata: [],
      queryparam: {},
      historytrade: {
        startDate: "",
        endDate: "",
        minAmt: "",
        maxAmt: "",
        tradeCurrency: "",
        accountType: "",
        payPayeeSign: ""
      },
      rules: {
        startDate: [
          {
            required: true,
            message: "请选择你要查询开始日期时间段",
            tigger: "chagne"
          }
        ],
        endDate: [
          {
            required: true,
            message: "请选择你要查询结束日期时间段",
            tigger: "chagne"
          }
        ],
        minAmt: [
          { required: true, message: "请输入金额下限", tigger: "blur" },
          { validator: checkminAmt, trigger: "blur" }
        ],
        maxAmt: [
          { required: true, message: "金额上限不可为空！", trigger: "blur" },
          { validator: checkminAmt, trigger: "blur" }
        ],
        tradeCurrency: [
          { required: true, message: "请选择交易币种", trigger: "blur" }
        ],
        accountType: [
          { required: true, message: "请选择收付标志", trigger: "blur" }
        ],
        payPayeeSign: [
          { required: true, message: "请选择收付标志", trigger: "change" }
        ]
      },
      options: [
        {
          value: "CHF",
          label: "法国法郎"
        },
        {
          value: "AUD",
          label: "澳大利亚元"
        },
        {
          value: "EUR",
          label: "欧元"
        },
        {
          value: "JPY",
          label: "日元"
        },
        {
          value: "CHF",
          label: "瑞士法郎"
        },
        {
          value: "SGD",
          label: "新加坡元"
        },
        {
          value: "CAD",
          label: "加拿大元"
        },
        {
          value: "CNY",
          label: "人民币"
        },
        {
          value: "USD",
          label: "美元"
        },
        {
          value: "GBP",
          label: "英镑"
        },
        {
          value: "HKD",
          label: "港元"
        },
        {
          value: "SEK",
          label: "瑞典克朗"
        }
      ],
      options1: [
        {
          value: "9",
          label: "资金监管账户"
        }
      ],
      options2: [
        {
          value: "0",
          label: "全部"
        },
        {
          value: "1",
          label: "付"
        },
        {
          value: "2",
          label: "收"
        }
      ]
    };
  },
  methods: {
    queryflowings(formName) {
      const _this = this;
      _this.flowingdata = [];
      _this.$refs[formName].validate(valid => {
        if (valid) {
          _this.queryparam = {
            merchantNo: window.merchantNo,
            startDate: _this.$moment(_this.historytrade.startDate).format("YYYYMMDD"),
            endDate: _this.$moment(_this.historytrade.endDate).format("YYYYMMDD"),
            minAmt: _this.historytrade.minAmt,
            maxAmt: _this.historytrade.maxAmt,
            tradeCurrency: _this.historytrade.tradeCurrency,
            accountType: _this.historytrade.accountType,
            payPayeeSign: _this.historytrade.PayPayeeSign
          };
          // console.log(JSON.stringify(_this.queryparam));
          historytrade(_this.queryparam).then(res => {
            console.log(res);
            _this.historydata = res.cycles;
          });
        }
      });
    }
  }
};
</script>
