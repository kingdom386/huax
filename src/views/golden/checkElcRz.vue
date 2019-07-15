<template>
  <div class="golden">
    <el-form ref="elcrz" :model="elcrz" inline label-width="100px">
      <el-row>
        <el-col :span="7">
          <el-form-item label="交易会员代码:" prop="tradeMemCode">
            <el-input placeholder="交易会员代码" clearable v-model="elcrz.tradeMemCode"></el-input>
          </el-form-item>
        </el-col>
        <el-col :span="7">
          <el-form-item label="交易会员名称:" prop="tradeMemBerName">
            <el-input placeholder="交易会员名称" clearable v-model="elcrz.tradeMemBerName"></el-input>
          </el-form-item>
        </el-col>
        <el-col :span="7">
          <el-form-item label="子账号:" prop="subAccount">
            <el-input placeholder="子账号" clearable v-model="elcrz.subAccount"></el-input>
          </el-form-item>
        </el-col>
        <el-col :span="7">
          <el-form-item label="币种标识:" prop="currency">
            <el-select v-model="elcrz.currency" filterable clearable placeholder="请选择">
              <el-option
                v-for="(item, index) in options"
                :key="index"
                :label="item.label"
                :value="item.value"
              ></el-option>
            </el-select>
          </el-form-item>
        </el-col>
        <el-col :offset="1" :span="4">
          <el-form-item>
            <el-button type="primary" @click="initdom">查询</el-button>
          </el-form-item>
        </el-col>
      </el-row>
    </el-form>
    <div class="tables">
      <el-table :data="tabledata" stripe border style="width: 100%">
        <el-table-column label="跨行收款账号" prop="othBankPayeeSubAcc" width="120"></el-table-column>
        <el-table-column label="跨行收款账号户名" prop="othBankPayeeSubAccName"></el-table-column>
        <el-table-column width="200px" label="跨行收款账号清算行名称" prop="othBankPayeeSubAccSetteName"></el-table-column>
        <el-table-column label="备注1" prop="Remark1"></el-table-column>
        <el-table-column label="备注2" prop="Remark2"></el-table-column>
        <el-table-column label="备注3" prop="Remark3"></el-table-column>
        <el-table-column label="备注4" prop="Remark4"></el-table-column>
        <el-table-column label="备注5" prop="Remark5"></el-table-column>
      </el-table>
    </div>
    <!-- tables -->
  </div>
</template>

<script>
import { queryElectronic } from "@/api/table/subcounttable";
import cookie from "js-cookie";

export default {
  data() {
    return {
      tabledata: [],
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
      elcrz: {
        tradeMemCode: "",
        tradeMemBerName: "",
        subAccount: "",
        currency: "CNY"
      },
      page: {
        currentPage: 1,
        pagesize: 20,
        total: 0
      },
      token: cookie.get("token")
    };
  },
  methods: {
    initdom() {
      const _this = this;
      const ls = {
        merchantNo: window.merchantNo,
        tradeMemCode: _this.elcrz.tradeMemCode,
        tradeMemBerName: _this.elcrz.tradeMemBerName,
        subAccount: _this.elcrz.subAccount,
        currency: _this.elcrz.currency
      };
      queryElectronic(ls).then(res => {
        console.log(res);
      });
    }
  }
};
</script>

<style lang="scss" scoped>
.tables {
  clear: both;
}
.clearfix {
  margin-top: 20px;
  text-align: right;
  overflow: hidden;
}
</style>

<style scoped>
.golden /deep/ .el-date-editor.el-input {
  width: 192px;
}
</style>
