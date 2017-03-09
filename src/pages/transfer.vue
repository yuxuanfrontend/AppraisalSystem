<style lang="css">
</style>
<template lang="html">
  <div class="apply">
    <common-menu></common-menu>
    <h4 class="title">仓库调拨</h4>
    <div class="main-apply">
        <div class="row">
          <div class="input-box col-xs-4">
            <p>调拨入库单号</p>
            <div class="input-group w50">
              <input type="text" class="form-control" name="" placeholder="调拨入库单号" v-model="tranferNo">
              <span class="input-group-btn">
                <button class="btn btn-default" type="button" @click="transfer_search">查询</button>
              </span>
            </div>
          </div>
          <div class="input-box col-xs-4">
            <p>提货车牌号</p>
            <div class="input-group">
              <select class="form-control w50 bd-radius" v-model="vehicleNo">
                <option v-for="(vehicle,index) in vehicles" :value="index+1">{{ vehicle.vehicleId }}</option>
              </select>
            </div>
          </div>
          <div class="input-box col-xs-4">
            <p>交接人&提货人</p>
            <div class="input-group">
              <select class="form-control w30 bd-radius" v-model="carrierNo">
                <option v-for="carrier in carriers" :value="carrier.uid">
                  {{ carrier.uname }}
                </option>
              </select>
              <select class="form-control w30 bd-radius" v-model="receiverNo" >
                <option v-for="receiver in receivers" :value="receiver.uid">
                  {{ receiver.uname }}
                </option>
              </select>
            </div>
          </div>
        </div>
        <div class="row">
          <div class="input-box col-xs-4">
            <p>登记日期</p>
            <div class="input-group">
              <input type="text" name="" class="form-control w30" value="" readonly v-model="transfer_date">
              <input type="text" name="" class="form-control w30" value="" readonly v-model="transfer_time">
            </div>
          </div>
          <div class="input-box col-xs-4">
            <p>车辆后门&侧门铅封条</p>
            <div class="input-group">
              <input type="text" name="" class="form-control w30 bd-radius" placeholder="后门铅封号" v-model="seal_no1">
              <input type="text" name="" class="form-control w30 bd-radius" placeholder="侧门铅封号" v-model="seal_no2">
            </div>
          </div>
          <div class="input-box col-xs-4">
            <p>出库仓库&入库仓库</p>
            <div class="input-group">
              <select class="form-control w30 bd-radius" v-model="outStoreNo">
                <option v-for="store in stores" :value="store.id" >{{ store.name }}</option>
              </select>
              <select class="form-control w30 bd-radius" v-model="inStoreNo">
                <option v-for="store in stores" :value="store.id" >{{ store.name }}</option>
              </select>
            </div>
          </div>
        </div>
        <table-editable :titles="tableTitles" ref="oneTable" :data="boxes" :keys="boxkeys" :is-calculate="true" :is-editable="true" :calculable-key="['invQty']" @delete-line="deleteBoxes" @add-line="addBox">
        </table-editable>
        <div class="btn-box" role="group">
          <button type="button" class="btn btn-default" @click="transfer_new">新单据</button>
          <button type="button" class="btn btn-default" @click="$refs.oneTable.deleteLine()">删除行</button>
          <button type="button" class="btn btn-default" @click="$refs.oneTable.addLine()">增加行</button>
          <button type="button" class="btn btn-default" @click="selectBox">仓库箱号</button>
          <button type="button" class="btn btn-default" @click="transfer_save">保存</button>
          <!-- <button type="button" class="btn btn-default"></button> -->
          <a class="btn btn-default" target="_blank" :href="'transfer.jsp?code='+ tranferNo">打印调拨单</a>
        </div>
        <transferDialog :show="dialogShow" @close-Dialog="closeDialog"></transferDialog>
    </div>
  </div>
</template>

<script>

import tableEditable from '../components/table-editable'
import commonMenu from '../components/menu'
import transferDialog from '../components/transfer-dialog'


export default {
  name: 'apply',
  data() {
    return {
      tableTitles: ['箱号', '流转单号', '藏品代码', '藏品名称', '数量'],
      boxes: [],
      boxkeys: ['boxNo', 'checkNo', 'productNo', 'productName', 'invQty'],
      receivers: [],
      carriers: [],
      vehicles: [],
      stores: [],
      receiverNo: 1,
      carrierNo: 1,
      vehicleNo: 1,
      outStoreNo: 1,
      inStoreNo: 1,
      tranferNo: '',
      transfer_date: '',
      transfer_time: '',
      seal_no1: '',
      seal_no2: '',
      dialogShow: false,
    }
  },
  components: {
    tableEditable,
    commonMenu,
    transferDialog,
  },
  mounted() {
    this.$request('get', 'api/v1/allocates/initialize')
    .then(
      (res) => {
        console.log(res)
        this.receivers = res.body.receivers
        this.carriers = res.body.carriers
        this.vehicles = res.body.vehicles
        this.stores = res.body.stores
      },
    )
  },
  methods: {
    selectBox() {
      this.dialogShow = true
    },
    closeDialog() {
      this.dialogShow = false
    },
    transfer_new() {
      this.$request('get', 'api/v1/handovers/currentDateTime')
      .then(
        (res) => {
          this.tranferNo = ''
          this.seal_no1 = ''
          this.seal_no2 = ''
          this.transfer_date = res.body.handoverDate
          this.transfer_time = res.body.handoverTime
          this.trboxes.splice(0, this.trboxes.length)
          this.boxes.splice(0, this.boxes.length)
        },
      )
    },
    transfer_save() {
      this.$request('post', '/api/v1/allocates/allocate')
      .send({
        receiveUser: this.receiverNo,
        deliveryUser: this.carrierNo,
        vehicleNo: this.vehicleNo,
        invInstore: this.storeNo,
        invDate: this.transfer_date,
        invTimeStr: this.transfer_time,
        sealNo1: this.seal_no1,
        sealNo2: this.seal_no2,
        inventoryDetails: this.boxes,
      })
      .then(
        (res) => {
          this.tranferNo = res.text
        },
      )
    },
    transfer_search() {
      this.$request('get', `api/v1/allocates/allocate/${this.tranferNo}`)
      .then(
        (res) => {
          this.receiverNo = res.body.receiveUser
          this.carrierNo = res.body.deliveryUser
          this.vehicleNo = res.body.vehicleNo
          this.storeNo = res.body.invInstore
          this.transfer_date = res.body.invDate
          this.transfer_time = res.body.invTimeStr
          this.seal_no1 = res.body.sealNo1
          this.seal_no2 = res.body.sealNo2
          this.boxes = res.body.inventoryDetails
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
        productName: '',
        invQty: 0,
      })
    },
  },
}
</script>
