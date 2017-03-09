<style lang="css" scoped>
.table-box tr td,
.table-box tr th{
  text-align: center!important;
}
</style>
<template lang="html">
  <div class="apply">
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
                <button class="btn btn-info bd-radius w30" type="button" @click="selectProduct">打印</button>
              </div>
            </div>
          </div>
          <div class="row">
            <div class="input-box col-xs-4">
              <div class="input-group">
                <label for="">存放仓库</label>
                <select class="form-control w50 bd-radius">
                  <option>全部</option>
                  <option>中工美</option>
                  <option>民生银行</option>
                </select>
              </div>
            </div>
          </div>
        </div>
    </div>
    <div class="table-box">
      <table class="table table-striped text-align">
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
    <distributiondialog></distributiondialog>
  </div>
</template>

<script>

// import _ from 'lodash'
import moment from 'moment'
// import Flatpickr from 'flatpickr'
// import datePickerZh from 'flatpickr/dist/l10n/zh'

import distributiondialog from '../components/distribution-dialog'

export default {
  name: 'apply',
  data() {
    return {
      tableBoxTitles: ['仓库名称', '提货日期', '产品总箱数'],
      boxTrDataArr: ['中工美', '2017.02-24', '12'],
      suns: ['合计', '', '12'],

      startDate: moment().format('YYYY-MM-DD'),
      endDate: moment().format('YYYY-MM-DD'),
    }
  },
  components: {
    distributiondialog,
  },
  mounted() {
    // new Flatpickr(this.$refs.startdate)
    // new Flatpickr(this.$refs.enddate)
  },
  methods: {
    selectProduct() {
      // this.$store.commit('toggleSelectDialog')
    },
  },
}
</script>
