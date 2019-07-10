<template>
  <div class="subcount">
    <el-form ref="subcount" :model="subcount" :inline="true" label-width="100px">
      <el-row>
        <el-col :span="7">
          <el-form-item label="开始日期:" prop="StartTimes">
            <el-date-picker
              class="datepicker"
              v-model="subcount.StartTimes"
              type="date"
              placeholder="选择日期"
            ></el-date-picker>
          </el-form-item>
        </el-col>
        <el-col :span="7">
          <el-form-item label="截止日期:" prop="EndTimes">
            <el-date-picker v-model="subcount.EndTimes" type="date" placeholder="选择日期"></el-date-picker>
          </el-form-item>
        </el-col>
        <el-col :span="7">
          <el-form-item label="子账号:" prop="SubAccount">
            <el-input placeholder="子账号" maxlength="20" clearable v-model="subcount.SubAccount"></el-input>
          </el-form-item>
        </el-col>
      </el-row>
      <el-row>
        <el-col :span="7">
          <el-form-item label="上级会员代码:" prop="GradeCode">
            <el-input placeholder="上级会员代码" maxlength="10" clearable v-model="subcount.GradeCode"></el-input>
          </el-form-item>
        </el-col>
        <el-col :span="7">
          <el-form-item label="交易会员代码:" prop="MemBerCode">
            <el-input v-model="subcount.MemBerCode" clearable></el-input>
          </el-form-item>
        </el-col>
        <el-col :span="7">
          <el-form-item>
            <el-button type="primary" @click="checkform('subcount')">查询</el-button>
            <el-button type="danger" @click="resetform('subcount')">重置</el-button>
          </el-form-item>
        </el-col>
      </el-row>
    </el-form>
    <div class="tables">
      <el-table :data="tabledata" stripe border style="width: 100%">
        <el-table-column label="序号" prop="orders" width="100px"></el-table-column>
        <el-table-column width="150px" label="子账号" prop="subAccount"></el-table-column>
        <el-table-column label="交易会员名称" prop="tradeMemBerName"></el-table-column>
        <el-table-column width="100px" label="交易会员代码" prop="memBerCode"></el-table-column>
        <el-table-column label="交易会员级别" prop="tradeMemBerGrade"></el-table-column>
        <el-table-column label="上级会员代码" prop="gradeCode"></el-table-column>
        <el-table-column label="商户代码" prop="merchantNo"></el-table-column>
        <el-table-column label="子账号开户时间" prop="subAccountTime"></el-table-column>
        <el-table-column label="子账户当前状态">
          <template slot-scope="scope">
            <span v-if="scope.row.subAccountState === '0'">
              <el-tag type="success">正常</el-tag>
            </span>
            <span v-else-if="scope.row.subAccountState === '1'">
              <el-tag type="info">禁用</el-tag>
            </span>
            <span v-else-if="scope.row.subAccountState === '2'">
              <el-tag type="danger">销户</el-tag>
            </span>
            <span v-else></span>
          </template>
        </el-table-column>
        <el-table-column label="联系人" prop="contact"></el-table-column>
        <el-table-column label="联系电话" prop="contactPhone"></el-table-column>
        <el-table-column label="手机号码" prop="phone"></el-table-column>
        <el-table-column label="联系地址" prop="contactAddr"></el-table-column>
        <el-table-column label="法人姓名" prop="businessName"></el-table-column>
        <el-table-column label="证件类型">
          <template slot-scope="scope">
            <span v-if="scope.row.papersType === '10'">
              <el-tag type="info">身份证</el-tag>
            </span>
            <span v-else-if="scope.row.papersType === '11'">
              <el-tag type="info">护照</el-tag>
            </span>
            <span v-else-if="scope.row.papersType === '12'">
              <el-tag type="info">军官证</el-tag>
            </span>
            <span v-else-if="scope.row.papersType === '13'">
              <el-tag type="info">士兵证</el-tag>
            </span>
            <span v-else-if="scope.row.papersType === '14'">
              <el-tag type="info">回乡证</el-tag>
            </span>
            <span v-else-if="scope.row.papersType === '15'">
              <el-tag type="info">户口本</el-tag>
            </span>
            <span v-else-if="scope.row.papersType === '16'">
              <el-tag type="info">营业执照</el-tag>
            </span>
            <span v-else-if="scope.row.papersType === '17'">
              <el-tag type="info">组织机构代码证</el-tag>
            </span>
            <span v-else-if="scope.row.papersType === '19'">
              <el-tag type="info">外国护照</el-tag>
            </span>
            <span v-else-if="scope.row.papersType === '20'">
              <el-tag type="info">其他</el-tag>
            </span>
            <span v-else></span>
          </template>
        </el-table-column>
        <el-table-column width="155px" label="证件代码" prop="papersCode"></el-table-column>
        <el-table-column label="是否需要短信通知" prop="isMessager">
          <template slot-scope="scope">
            <span v-if="scope.row.isMessager === '1'">
              <el-tag type="primary">需要</el-tag>
            </span>
            <span v-else-if="scope.row.isMessager === '2'">
              <el-tag type="danger">不需要</el-tag>
            </span>
            <span v-else></span>
          </template>
        </el-table-column>
        <el-table-column label="短信通知手机号码" prop="messagePhone"></el-table-column>
        <el-table-column width="160px" label="Email" prop="email"></el-table-column>
        <el-table-column label="备注1" prop="remark1"></el-table-column>
        <el-table-column label="备注2" prop="remark2"></el-table-column>
        <el-table-column label="备注3" prop="remark3"></el-table-column>
        <el-table-column label="备注4" prop="remark4"></el-table-column>
        <el-table-column label="备注5" prop="remark5"></el-table-column>
        <el-table-column fixed="right" label="操作" width="200px">
          <template slot-scope="scope">
            <el-button @click.native.prevent="checkRow(scope.row)" type="text">客户签约信息查询</el-button>
            <el-button @click.native.prevent="balanceRow(scope.row)" type="text">账户余额</el-button></br>
            <el-button @click.native.prevent="bindmoney(scope.row)" type="text">子账户绑定出入金查询</el-button></br>
            <el-button @click.native.prevent="tradesubcount(scope.row)" type="text">子账户交易明细</el-button>
            <!-- <el-button @click.native.prevent="countinterest(scope.row)" type="text">账户计息查询</el-button> -->
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
        :page-sizes="[20, 40, 60, 100]"
        :page-size="page.pagesize"
        layout="total, sizes, prev, pager, next, jumper"
        :total="page.total"
      ></el-pagination>
    </div>
    <!-- clearfix -->
    <el-dialog title="客户签约查询" :visible.sync="dialogTableVisible" width="90%">
      <custome :customeinfo="customeinfo"></custome>
    </el-dialog>
    <el-dialog title="子账户绑定出入金查询" :visible.sync="dialogTableBindVisible" width="90%">
      <money :moneyinfo='moneyinfo'></money>
    </el-dialog>
    <el-dialog title="账户余额查询" :visible.sync="dialogTableBalanceVisible"  width="90%">
      <balance :balanceinfo="balanceinfo"></balance>
    </el-dialog>
    <el-dialog title="子账户交易明细查询" :visible.sync="dialogTradeVisible"  width="90%">
      <trading :tradeinfo="tradeinfo"></trading>
    </el-dialog>
    <!-- <el-dialog title="账户计息查询" :visible.sync="dialogJxVisible"  width="90%">
      <countinterest :countinterestinfo="countinterestinfo"></countinterest>
    </el-dialog> -->
  </div>
</template>

<script>
import { checksubcount } from "@/api/table/subcounttable";
import cookie from "js-cookie";
import custome from "./checkCustome";
import money from './checkbBindMoney';
import balance from '@/views/others/checkcountbalance'
import trading from '@/views/others/checksubcountdetail'
import countinterest from '@/views/others/checkcountinterest'
import { thirtyday } from '@/utils/validate'

export default {
  components: {
    custome,
    money,
    balance,
    trading,
    countinterest
  },
  data() {
    return {
      tabledata: [],
      dialogTableVisible: false,
      dialogTableBindVisible: false,
      dialogTableBalanceVisible: false,
      dialogTradeVisible: false,
      dialogJxVisible: false,
      merchantNo: window.merchantNo,
      customeinfo: {},
      moneyinfo: {},
      balanceinfo: {},
      tradeinfo: {},
      subcount: {
        StartTimes: "",
        EndTimes: "",
        SubAccount: "",
        GradeCode: "",
        MemBerCode: ""
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
    const _this = this;
    let ls = { merchantNo: _this.merchantNo };
    checksubcount(ls).then(res => {
      _this.tabledata = res.cycles;
      _this.page.total = parseInt(res.count);
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
    // 查看row
    checkRow(rows) {
      this.customeinfo = {
        merchantNo: rows.merchantNo,
        papersCode: rows.papersCode
      }
      this.dialogTableVisible = true
    },
    bindmoney(row) {
      this.dialogTableBindVisible = true
      this.moneyinfo = {
        merchantNo: row.merchantNo,
        subAccount: row.subAccount,
        tradeMemCode: row.memBerCode
      }
    },
    balanceRow(row) {
      this.dialogTableBalanceVisible = true
      this.balanceinfo = {
        merchantNo: row.merchantNo,
        account: row.subAccount,
        memBerCode: row.memBerCode,
        accountType: 1
      }
    },
    tradesubcount(row) {
      this.dialogTradeVisible = true
      const date = new Date();
      this.tradeinfo = {
        merchantNo: row.merchantNo,
        tradeMemCode: row.memBerCode,
        startTimes: this.$moment(thirtyday()).format("YYYYMMDDHHss"),
        endTimes: this.$moment(date).format("YYYYMMDDHHss")
      }
    },
    countinterest(row) {
      this.dialogJxVisible = true
      this.countinterest = {
        merchantNo: row.merchantNo,
        tradeMemCode: row.memBerCode
      }
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
.subcount /deep/ .el-date-editor.el-input {
  width: 192px;
}
</style>
