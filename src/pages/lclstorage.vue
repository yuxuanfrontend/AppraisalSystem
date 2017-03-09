<style lang="scss">
</style>

<template lang="html">
  <div class="apply">
    <common-menu></common-menu>
    <h4 class="title">拼箱入库登记</h4>
    <div class="main-apply">
        <div class="row">
          <div class="input-box col-xs-4">
            <p>箱号</p>
            <div class="input-group w50">
              <input type="text" class="form-control" placeholder="查询拼箱号" v-model="lclId">
              <span class="input-group-btn">
                <button class="btn btn-default" @click="queryForm">查询</button>
              </span>
            </div>
          </div>
          <div class="input-box col-xs-4">
            <p>出库单号</p>
            <div class="input-group w50">
              <input type="text" class="form-control w70 bd-radius" readonly :value="outWareId">
            </div>
          </div>
          <div class="input-box col-xs-4">
            <p>入库仓库</p>
            <div class="input-group w50">
              <select v-model="selectWarehouse" class="form-control w70 bd-radius" readonly>
                <option v-for="warehouse in warehouses" :value="warehouse.id">{{warehouse.name}}</option>
              </select>
            </div>
          </div>
          <div class="input-box col-xs-4">
            <p>登记日期</p>
            <div class="input-group w50">
              <input type="text" class="form-control w70 bd-radius" readonly v-model="registerTime">
            </div>
          </div>
          <div class="input-box col-xs-4">
            <p>铅封号1</p>
            <div class="input-group w50">
              <input type="text" class="form-control w70 bd-radius" v-model="sealId1">
            </div>
          </div>
          <div class="input-box col-xs-4">
            <p>铅封号2</p>
            <div class="input-group w50">
              <input type="text" class="form-control w70 bd-radius" v-model="sealId2">
            </div>
          </div>
          <div class="input-box col-xs-4">
            <p>合格数量</p>
            <div class="input-group w50">
              <input type="text" class="form-control w70 bd-radius" readonly :value="qualifiedAmount">
            </div>
          </div>
          <div class="input-box col-xs-4">
            <p>艺术品代码</p>
            <div class="input-group w50">
              <input type="text" class="form-control" placeholder="查询艺术品" v-model="artworkCode">
              <span class="input-group-btn">
                <button class="btn btn-default" @click="queryArtwork">查询</button>
              </span>
            </div>
          </div>
          <div class="input-box col-xs-4">
            <p>艺术品名称&最小单位</p>
            <div class="input-group w50">
              <input type="text" class="form-control w50" readonly v-model="artworkName">
              <input type="text" class="form-control w20" readonly v-model="artworkUnit">
            </div>
          </div>
        </div>
        <table-editable :titles="tableTitles" ref="oneTable" :data="boxes" :keys="boxkeys" :is-calculate="true" :is-editable="true" :calculable-key="['handoverQty']" @delete-line="deleteBoxes" @add-line="addBox">
        </table-editable>

        <div class="vm-vertical-block">
          <button type="button" class="btn btn-default" @click="newForm">新单据</button>
          <button type="button" class="btn btn-default" @click="$refs.oneTable.deleteLine();">删除行</button>
          <button type="button" class="btn btn-default" @click="selectBoxes">选择总仓箱号</button>
          <button type="button" class="btn btn-default" @click="save">保存</button>
          <a class="btn btn-default" target="_blank" :href="'lclstorage.jsp?invno=' + lclId">打印拼箱入库登记单</a>
        </div>
    </div>
    <div class="row is-multiline" v-if="">
      <!-- <div class="control column is-1">
        <label class="label">入库单号</label>
        <input type="text" class="form-control is-disabled" readonly :value="inWareId">
      </div> -->
    </div>
  </div>
    <!-- <storage-lcl-dialog :artwork="artworkCode" ref="dialog"></storage-lcl-dialog> -->
</template>

<script>

// import _ from 'lodash'
// import moment from 'moment'
import tableEditable from '../components/table-editable'

import commonMenu from '../components/menu'
// import storageLclDialog from './storage-lcl-dialog'

// import tableEditable from '../../components/table-editable'

export default {
  name: 'storage-lcl',
  data() {
    return {
      dd: 123,
      formId: null,
      lclId: '',
      inWareId: '',
      outWareId: '',
      sealId1: '',
      sealId2: '',
      registerTime: '',
      isModify: true,
      tableTitles: ['箱号', '流转单号', '藏品代码', '藏品名称', '件数', '提货'],
      warehouses: [],
      selectWarehouse: 1,
      artworkCode: '',
      artworkName: '',
      artworkUnit: '',
      boxkeys: ['boxNo', 'checkNo', 'productNo', 'productName', 'invQty', 'handoverNo'],
      boxes: [],
    }
  },

  computed: {
    boxes() {
      return this.$store.state.warehouse.selectedBoxTable
    },

  },

  mounted() {

  },
  methods: {
    newForm() {

    },
    queryArtwork() {

    },
    queryForm() {

    },
    selectBoxes() {

    },
    save() {

    },
    deleteBoxes() {

    },
    addBox() {

    },
  },

  components: {
    tableEditable,
    commonMenu,
  },
}
</script>
