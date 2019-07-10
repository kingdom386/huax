<template>
  <el-table :data="tradedata" stripe border style="width: 100%">
    <el-table-column prop="account" label="账号"></el-table-column>
    <el-table-column prop="accountName" label="账户名称"></el-table-column>
    <el-table-column prop="subAccountMoney" label="子账户余额"></el-table-column>
    <el-table-column prop="freezeMoney" label="冻结金额"></el-table-column>
    <el-table-column prop="remark1" label="备注1"></el-table-column>
    <el-table-column prop="remark2" label="备注2"></el-table-column>
  </el-table>
</template>

<script>
import { checktradedetail } from '@/api/table/subcounttable'
export default {
  props: {
    tradeinfo: {
      type: Object,
      default: () => {
        return {}
      }
    }
  },
  data () {
    return {
      merchantNo: "",
      tradedata: []
    }
  },
  watch: {
    tradeinfo(news, old) {
      this.init(news)
    }
  },
  mounted() {
    this.init(this.tradeinfo)
  },
  methods: {
    init(obj) {
      const _this = this
      _this.tradedata = [];
      checktradedetail(obj).then(res => {
        console.log(res);
        _this.tradedata.push(res)
      })
    }
  }
}
</script>
