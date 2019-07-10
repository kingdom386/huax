<template>
  <el-table :data="balancedata" stripe border style="width: 100%">
    <el-table-column prop="account" label="账号"></el-table-column>
    <el-table-column prop="accountName" label="账户名称"></el-table-column>
    <el-table-column prop="subAccountMoney" label="子账户余额"></el-table-column>
    <el-table-column prop="freezeMoney" label="冻结金额"></el-table-column>
    <el-table-column prop="remark1" label="备注1"></el-table-column>
    <el-table-column prop="remark2" label="备注2"></el-table-column>
  </el-table>
</template>

<script>
import { checkbalance } from '@/api/table/subcounttable'
export default {
  props: {
    balanceinfo: {
      type: Object,
      default: () => {
        return {}
      }
    }
  },
  data () {
    return {
      merchantNo: "",
      balancedata: []
    }
  },
  watch: {
    balanceinfo(news, old) {
      this.init(news)
    }
  },
  mounted() {
    this.init(this.balanceinfo)
  },
  methods: {
    init(obj) {
      const _this = this
      _this.balancedata = [];
      checkbalance(obj).then(res => {
        _this.balancedata.push(res)
      })
    }
  }
}
</script>
