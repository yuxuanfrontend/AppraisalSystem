<style lang="css">
</style>
<template lang="html">
  <div class="apply">
    <common-menu></common-menu>
    <div class="panel panel-primary">
      <div class="panel-heading">配货</div>
        <div class="panel-body">
          <div class="row">
            <div class="input-box col-xs-4">
              <div class="input-group">
                <label for="">开始日期</label>
                <input type="text" class="form-control bd-radius" style="width:50%" ref="startdate" v-model="startDate" @input="">
              </div>
            </div>
            <div class="input-box col-xs-4">
              <div class="input-group">
                <label for="">结束日期</label>
                <input type="text" class="form-control bd-radius" style="width:50%" ref="enddate" v-model="endDate" @input="">
              </div>
            </div>
            <div class="input-box col-xs-4">
              <div class="input-group">
                <button class="btn btn-primary bd-radius w30" type="button">查询</button>
                <button class="btn btn-info bd-radius w30" type="button" @click="selectProduct">配货</button>
              </div>
            </div>
          </div>
          <div class="row">
            <div class="input-box col-xs-4">
              <div class="input-group">
                <label for="">客户账号</label>
                <input type="text" class="form-control bd-radius" style="width:50%" placeholder="请输入客户账号">
              </div>
            </div>
            <div class="input-box col-xs-4">
              <div class="input-group">
                <label for="">产品代码</label>
                <input type="text" class="form-control bd-radius" style="width:50%" placeholder="请输入产品代码">
              </div>
            </div>
          </div>
        </div>
    </div>
    <div class="table-box">
      <table class="table table-striped">
        <thead>
          <tr>
            <th v-for='titleBox in tableBoxTitles'>{{ titleBox }}</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td v-for='boxTrTxt in boxTrDataArr'>{{ boxTrTxt }}</td>
          </tr>
        </tbody>
        <tfoot>
          <tr>
            <th v-for='sun in suns'>{{ sun }}</th>
          </tr>
        </tfoot>
      </table>
    </div>
    <distributiondialog :show="dialogShow" @close-Dialog="closeDialog"></distributiondialog>
  </div>
</template>

<script>
import moment from 'moment'

import distributiondialog from '../components/distribution-dialog'

// import Flatpickr from 'flatpickr'
// import datePickerZh from 'flatpickr/dist/l10n/zh'
import commonMenu from '../components/menu'

export default {
  name: 'apply',
  data() {
    return {
      tableBoxTitles: ['提货日期', '客户账号', '客户姓名', '产品代码', '产品名称', '最小包装单位', '提货申请数量', '状态'],
      boxTrDataArr: ['00212', '2121', '00212', '2121', '航天钞', 1, 22, '未锁定'],
      suns: ['合计', '', '', '', '', '', 22, ''],
      dialogShow: false,

      startDate: moment().format('YYYY-MM-DD'),
      endDate: moment().format('YYYY-MM-DD'),
    }
  },
  components: {
    distributiondialog,
    commonMenu,
  },
  mounted() {
    // new Flatpickr(this.$refs.startdate)
    // new Flatpickr(this.$refs.enddate)
  },
  methods: {
    selectProduct() {
      this.dialogShow = true
    },
    closeDialog() {
      this.dialogShow = false
    },
  },
}
</script>
