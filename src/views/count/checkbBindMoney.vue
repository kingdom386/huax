<template>
  <div>
    <div class="tables">
      <el-table :data="binddata" stripe border style="width: 100%">
        <el-table-column label="序号" prop="order"></el-table-column>
        <el-table-column width="140px" label="子账号" prop="subAccount"></el-table-column>
        <el-table-column width="100px" label="交易会员代码" prop="tradeMemCode"></el-table-column>
        <el-table-column label="商户代码" prop="merchantNo"></el-table-column>
        <el-table-column label="出入金账户类别">
          <template slot-scope="scope">
            <el-tag type="primary" v-if="scope.row.outComeAccountType === '0'">出入金</el-tag>
            <el-tag type="primary" v-else-if="scope.row.outComeAccountType === '1'">白名单1</el-tag>
            <el-tag type="primary" v-else-if="scope.row.outComeAccountType === '2'">白名单2</el-tag>
            <el-tag type="primary" v-else>白名单3</el-tag>
          </template>
        </el-table-column>
        <el-table-column label="对公或对私">
          <template slot-scope="scope">
            <el-tag type="primary" v-if="scope.row.isOther === '1'">公司</el-tag>
            <el-tag type="success" v-else>个人</el-tag>
          </template>
        </el-table-column>
        <el-table-column label="卡号/账号标识">
          <template slot-scope="scope">
            <el-tag type="primary" v-if="scope.row.accountSign === '0'">卡号</el-tag>
            <el-tag type="success" v-else>账号</el-tag>
          </template>
        </el-table-column>
        <el-table-column label="是否跨行" prop="isOtherBank">
          <template slot-scope="scope">
            <el-tag type="primary" v-if="scope.row.accountSign === '0'">华夏银行</el-tag>
            <el-tag type="warning" v-else>其他银行</el-tag>
          </template>
        </el-table-column>
        <el-table-column label="结算账号户名" prop="settleAccountName"></el-table-column>
        <el-table-column width="150px" label="结算账号" prop="settleAccount"></el-table-column>
        <el-table-column label="支付系统行号" prop="payBank"></el-table-column>
        <el-table-column label="开户行名称" prop="bankName"></el-table-column>
        <el-table-column label="变更日期" prop="modifyDate"></el-table-column>
        <el-table-column label="授权完成时间" prop="ouathEndTimes"></el-table-column>
        <el-table-column label="绑定状态" prop="linkState">
          <template slot-scope="scope">
            <el-tag type="primary" v-if="scope.row.accountSign === '0'">正常</el-tag>
            <el-tag type="danger" v-else>已删除</el-tag>
          </template>
        </el-table-column>
        <el-table-column label="授权渠道" prop="ouathWay"></el-table-column>
        <el-table-column label="备注1" prop="remark1"></el-table-column>
        <el-table-column label="备注2" prop="Rremark2"></el-table-column>
      </el-table>
    </div>
    <!-- tables -->
  </div>
</template>

<script>
import { checkbindmoney } from "@/api/table/subcounttable";
import cookie from "js-cookie";

export default {
  props: {
    moneyinfo: {
      type: Object,
      default: () => {
        return {};
      }
    }
  },
  data() {
    return {
      binddata: [{}],
      token: cookie.get("token")
    };
  },
  watch: {
    moneyinfo(news, olds) {
      this.checkRow(news);
    }
  },
  mounted() {
    this.checkRow(this.moneyinfo);
  },
  methods: {
    // 查询
    checkform(formName) {
      console.log(JSON.stringify(this.subcount));
    },
    // 重置
    resetform(formName) {
      this.$refs[formName].resetFields();
    },
    checkRow(infos) {
      const _this = this;
      checkbindmoney(_this.moneyinfo).then(res => {
        _this.binddata = res.cycles || [];
      });
    }
  }
};
</script>

<style lang="scss" scoped>
.tables {
  clear: both;
}
</style>
