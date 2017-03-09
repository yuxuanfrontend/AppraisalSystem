<style lang="css">
.table-box .table-center tr th,
.table-box .table-center tr td {
  text-align: center!important;
}
</style>
<template lang="html">
  <div class="apply">
    <common-menu></common-menu>
    <h4 class="title">装箱入库</h4>
    <div class="main-apply">
        <div class="row">
          <div class="input-box col-xs-4">
            <p>箱号</p>
            <div class="input-group w50">
              <input type="text" class="form-control" placeholder="箱号" v-model="boxNo" id="boxId">
              <span class="input-group-btn">
                <button class="btn btn-default" type="button" @click="boxNo_search">查询</button>
              </span>
            </div>
          </div>
          <div class="input-box col-xs-4">
            <p>铅封号1</p>
            <div class="input-group w50">
              <input type="text" class="form-control bd-radius" placeholder="铅封号1" >
            </div>
          </div>
          <div class="input-box col-xs-4">
            <p>铅封号2</p>
            <div class="input-group w50">
              <input type="text" class="form-control bd-radius" placeholder="铅封号2">
            </div>
          </div>
        </div>
        <div class="row">
          <div class="input-box col-xs-4">
            <p>登记日期&状态</p>
            <div class="input-group">
              <input type="text" name="" class="form-control w30"readonly>
              <input type="text" name="" class="form-control w30"readonly>
            </div>
          </div>
          <div class="input-box col-xs-4">
            <p>鉴定流转单号</p>
            <div class="input-group w80">
              <input type="text" name="" class="form-control">
              <span class="input-group-btn">
                <button class="btn btn-default" type="button" @click="outbound_search">查询</button>
                <button class="btn btn-default" type="button" @click="outbound_search">增加到列表</button>
              </span>
            </div>
          </div>
          <div class="input-box col-xs-4">
            <p>入库仓库</p>
            <div class="input-group">
              <select class="form-control w50 bd-radius">
                <option>引立仓库</option>
              </select>
            </div>
          </div>
        </div>
        <div class="row">
          <div class="input-box col-xs-4">
            <p>艺术品名称&最小单位</p>
            <div class="input-group">
              <input type="text" class="form-control w30">
              <input type="text" class="form-control w20">
            </div>
          </div>
          <div class="input-box col-xs-4">
            <p>艺术品代码</p>
            <div class="input-group">
              <input type="text" class="form-control w50 bd-radius">
            </div>
          </div>
          <div class="input-box col-xs-4">
            <p>合格数量&累计入库&本次入库</p>
            <div class="input-group">
              <input type="text" class="form-control w30">
              <input type="text" class="form-control w30">
              <input type="text" class="form-control w30">
            </div>
          </div>
        </div>
        <!-- <div class="table-box">
          <table class="table table-striped table-center">
            <thead>
              <tr>
                <th v-for='titleBox in tableBoxTitles'>{{ titleBox }}</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="trbox in trboxes">
                <td v-for="tdbox in tdboxes">{{ trbox[tdbox] }}</td>
              </tr>
            </tbody>
          </table>
        </div> -->
          <table-editable :titles="tableTitles" ref="oneTable" :data="boxes" :keys="boxkeys" :is-calculate="true" :is-editable="true" :calculable-key="['invQty']" @delete-line="deleteBoxes" @add-line="addBox">
        </table-editable>
        <div class="btn-box" role="group">
          <button type="button" class="btn btn-default" @click="outbound_new">新单据</button>
          <button type="button" class="btn btn-default" @click="$refs.oneTable.deleteLine()">删除行</button>
          <button type="button" class="btn btn-default" @click="$refs.oneTable.addLine()">增加行</button>
          <button type="button" class="btn btn-default" @click="outbound_save">保存</button>
          <a class="btn btn-default" target="_blank" :href="'outbound_print.jsp?code='+ outbound_no">打印出库单</a>
        </div>
    </div>
    <!-- <tabledialog></tabledialog> -->
  </div>
</template>

<script>
import moment from 'moment'
import tableEditable from '../components/table-editable'
import commonMenu from '../components/menu'

export default {
  name: 'apply',
  data() {
    return {
      tableBoxTitles: ['产品代码', '产品名称', '单位', '提货数量', '备注'],
      trboxes: [],
      tdboxes: ['productNo', 'productName', 'productUnit', 'handoverQty', 'handoverRemark'],
      tableTitles: ['箱号', '流转单号', '产品代码', '提货数量'],
      boxes: [],
      boxkeys: ['boxNo', 'checkNo', 'productNo', 'invQty'],
      outbound_no: '',
      handover_no: '',
      custom_no: '',
      custom_name: '',
      outbound_date: '',
      outbound_time: '',
      notify_date: '',
    }
  },
  components: {
    tableEditable,
    commonMenu,
  },
  mounted() {
  },
  methods: {
    outbound_new() {
      this.$request('get', 'api/v1/handovers/currentDateTime')
      .then(
        (res) => {
          this.outbound_no = ''
          this.handover_no = ''
          this.custom_no = ''
          this.custom_name = ''
          this.notify_date = ''
          this.outbound_date = res.body.handoverDate
          this.outbound_time = res.body.handoverTime
          this.trboxes.splice(0, this.trboxes.length)
          this.boxes.splice(0, this.boxes.length)
        },
      )
    },
    boxNo_search() {
      this.$nextTick(
        () => {
          document.getElementById('boxId').focus()
        },
      )
    },
    outbound_save() {
      this.$request('post', 'api/v1/inventories/inventory')
      .send({
        handoverNo: this.handover_no,
        deliveryUser: this.custom_name,
        invDate: this.outbound_date,
        invTimeStr: this.outbound_time,
        inventoryDetails: this.boxes,
      })
      .then(
        (res) => {
          this.outbound_no = res.text
        },
      )
    },
    handover_search() {
      this.$request('get', `api/v1/handovers/handover/${this.handover_no}`)
      .then(
        (res) => {
          this.trboxes = res.body.handoverDetails
          this.custom_no = res.body.customNo
          this.custom_name = res.body.customName
          this.notify_date = moment(res.body.notifyData).format('YYYY-MM-DD')
        },
      )
    },
    deleteBoxes(indexes) {
      for (let i = 0; i < indexes.length; i += 1) {
        this.boxes.splice(indexes[i], 1)
      }
    },
    addBox() {
      this.boxes.push({
        boxNo: '',
        checkNo: '',
        productNo: '',
        invQty: '',
      })
    },
  },
}
</script>
