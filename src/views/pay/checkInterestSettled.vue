<template>
  <div>
    <el-form ref="settled" :model="settled" :inline="true" label-width="120px">
      <el-row>
        <el-col :span="7">
          <el-form-item label="批量结息流水号:" prop="BatchAccintPayCode">
            <el-input placeholder="批量结息流水号" v-model="settled.BatchAccintPayCode"></el-input>
          </el-form-item>
        </el-col>
        <el-col :span="6">
          <el-form-item>
            <el-button type="primary" @click="checkform('settled')">查询</el-button>
            <el-button type="danger" @click="resetform('settled')">重置</el-button>
          </el-form-item>
        </el-col>
      </el-row>
    </el-form>
    <div class="tables">
      <el-table :data="tabledata" stripe border style="width: 100%">
        <el-table-column label="子账号" prop="SubAccount" width="120"></el-table-column>
        <el-table-column label="币种" prop="Currency"></el-table-column>
        <el-table-column label="交易会员名称" prop="TradeMemBerName"></el-table-column>
        <el-table-column label="交易会员代码" prop="MemBerCode"></el-table-column>
        <el-table-column label="交易会员级别" prop="TradeMemBerGrade"></el-table-column>
        <el-table-column label="上级会员代码" prop="GradeCode"></el-table-column>
        <el-table-column label="商户代码" prop="MerchantNo"></el-table-column>
        <el-table-column label="交易会员性质" prop="TradeMemberProperty">
          <template slot-scope="scope">
            <span v-if="scope.row.TradeMemberProperty === 0">
              <el-tag type="success">企业</el-tag>
            </span>
            <span v-else-if="scope.row.TradeMemberProperty === 1">
              <el-tag type="info">个人</el-tag>
            </span>
            <span v-else></span>
          </template>
        </el-table-column>
        <el-table-column label="联系人" prop="Contact"></el-table-column>
        <el-table-column label="联系电话" prop="ContactPhone"></el-table-column>
        <el-table-column label="手机号码" prop="Phone"></el-table-column>
        <el-table-column label="联系地址" prop="ContactAddr"></el-table-column>
        <el-table-column label="法人姓名" prop="BusinessName"></el-table-column>
        <el-table-column label="证件类型" prop="PapersType"></el-table-column>
        <el-table-column label="证件代码" prop="PapersCode"></el-table-column>
        <el-table-column label="是否需要短信通知">
          <template slot-scope="scope">
            <el-tag v-if="scope.row.IsMessager === 1">需要</el-tag>
            <el-tag v-if="scope.row.IsMessager === 2">不需要</el-tag>
            <span v-else></span>
          </template>
        </el-table-column>
        <el-table-column label="短信通知手机号码" prop="MessagePhone"></el-table-column>
        <el-table-column label="Email" prop="Email"></el-table-column>
        <el-table-column label="结算账号" prop="SettleAccount"></el-table-column>
        <el-table-column label="结算账号户名" prop="SettleAccountBank"></el-table-column>
        <el-table-column label="支付系统行号" prop="PayBank"></el-table-column>
        <el-table-column label="个人/对公类型">
          <template slot-scope="scope">
            <el-tag v-if="scope.row.OthersType === 1">对公</el-tag>
            <el-tag v-else-if="scope.row.OthersType === 2">个人</el-tag>
            <span v-else></span>
          </template>
        </el-table-column>

        <el-table-column label="卡号/账号标识">
          <template slot-scope="scope">
            <el-tag v-if="scope.row.SignBank === 0">卡号</el-tag>
            <el-tag v-else-if="scope.row.SignBank === 2">账号</el-tag>
            <span v-else></span>
          </template>
        </el-table-column>
        <el-table-column label="跨行标识">
          <template slot-scope="scope">
            <span v-if="scope.row.SignOtherBank === 0">本行</span>
            <span v-else>{{scope.row.SignOtherBank}}</span>
          </template>
        </el-table-column>
        <el-table-column label="签约状态">
          <template slot-scope="scope">
            <span v-if="scope.row.SignState === 1">失败</span>
            <span v-else-if="scope.row.SignState === 2">成功</span>
            <span v-else-if="scope.row.SignState === 3">未查询</span>
            <span v-else></span>
          </template>
        </el-table-column>
        <el-table-column label="是否授权">
          <template slot-scope="scope">
            <el-tag type="info" v-if="scope.row.IsOuath === 1">不需要授权</el-tag>
            <el-tag type="danger" v-if="scope.row.IsOuath === 2">未授权</el-tag>
            <el-tag type="success" v-if="scope.row.IsOuath === 3">已授权</el-tag>
            <span v-else></span>
          </template>
        </el-table-column>
        <el-table-column label="授权日期" prop="OuathTimes"></el-table-column>
        <el-table-column label="备注1" prop="Remark1"></el-table-column>
        <el-table-column label="备注2" prop="Remark2"></el-table-column>
        <el-table-column fixed="right" label="操作" width="160">
          <template slot-scope="scope">
            <el-button @click.native.prevent="deleteRow(scope.$index, tabledata)" type="text">移除</el-button>
            <el-button type="text">查看</el-button>
            <el-button type="text">编辑</el-button>
          </template>
        </el-table-column>
      </el-table>
    </div>
    <!-- tables -->
    <div class="clearfix">
      <el-pagination
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
        :current-page="page.currentPage"
        :page-sizes="[10, 20, 50, 100]"
        :page-size="page.pagesize"
        layout="total, sizes, prev, pager, next, jumper"
        :total="page.total"
      ></el-pagination>
    </div>
    <!-- clearfix -->
  </div>
</template>

<script>
import { checkinterestsettled } from "@/api/table/subcounttable";
import cookie from "js-cookie";

export default {
  data() {
    return {
      tabledata: [],
      settled: {
        BatchAccintPayCode: ""
      },
      page: {
        currentPage: 1,
        pagesize: 20,
        total: 0
      },
      token: cookie.get("token")
    };
  },
  created() {
    // const _this = this;
    checkinterestsettled().then(res => {
      console.log(res);
    });
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
    deleteRow(index, rows) {
      const _this = this;
      _this
        .$confirm("确认删除这条数据么？", "提示", { type: "warning" })
        .then(() => {
          _this.$message({ message: "删除成功！", type: "success" });
          rows.splice(index, 1);
        })
        .catch(errro => {
          _this.$message({ message: "删除失败，用户已取消！", type: "info" });
        });
    },
    handleSizeChange(val) {
      this.page.pagesize = val;
    },
    handleCurrentChange(val) {
      this.page.currentPage = val;
    }
  }
};
</script>

<style lang="scss" scoped>
.tables{
  clear: both;
}
.clearfix {
  margin-top: 20px;
  text-align: right;
  overflow: hidden;
}
</style>
