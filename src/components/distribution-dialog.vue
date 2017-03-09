<style lang="css">
.modal-background {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0,0,0,.5);
}

.flatpickr-calendar{
  z-index: 9999;
}

.is-active {
  display: block;
}
</style>

<template lang="html">
  <!-- <div class="modal"> -->
  <div class="modal" :class="{'is-active':show }">
    <div class="modal-background" @click="closeDialog"></div>
    <div class="modal-dialog modal-lg">
      <div class="modal-content">
        <div class="modal-header">
          <button type="button" class="close" data-dismiss="modal" aria-hidden="true" @click="closeDialog">×</button>
          <h4 class="modal-title" id="myModalLabel">箱号清单</h4>
        </div>
        <div class="modal-body" style="overflow:hidden;">
          <div class="col-xs-6">
            <input type="text" class="form-control" style="width:70%" ref="startdate" v-model="startDate" @input="queryBoxes">
            <br>
          </div>
          <div class="col-xs-6">
            <input type="text" class="form-control" style="width:70%" ref="enddate" v-model="endDate" @input="queryBoxes">
            <br>
          </div>
          <div class="col-xs-12">
            <table-editable :titles="tableTitles" ref="oneTable" :data="boxes" :keys="boxkeys" :is-calculate="true" :is-editable="true" >
          </table-editable>
          </div>

          <div style="text-align:right;">
            <button type="button" class="btn btn-default" @click="queryBoxes">查询</button>
            <button type="button" class="btn btn-default" @click="$refs.twoTable.confirmSelect()">确定</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

// import _ from 'lodash'
import moment from 'moment'
import Flatpickr from 'flatpickr'
import datePickerZh from 'flatpickr/dist/l10n/zh'

import tableEditable from './table-editable'

Flatpickr.localize(datePickerZh.zh)

export default {
  name: 'storage-dialog',

  components: {
    tableEditable,
  },

  data() {
    return {
      tableTitles: ['箱号', '流转单号', '藏品代码', '藏品名称', '件数', '提货'],
      tdboxes: ['productNo', 'productName', 'productUnit', 'handoverQty', 'handoverRemark'],
      boxes: [],
      boxkeys: ['boxNo', 'checkNo', 'productNo', 'invQty'],
      isActive: true,
      startDate: moment().format('YYYY-MM-DD'),
      endDate: moment().format('YYYY-MM-DD'),
    }
  },

  // props: {
  //   artwork: {
  //     required: true,
  //   },
  // },
  props: {
    show: {
      require: true,
      type: Boolean,
    },
  },
  computed: {
    // isActive() {
    //   return this.$store.state.warehouse.isDialogActive
    // },
  },

  mounted() {
    // new Flatpickr(this.$refs.startdate)
    // new Flatpickr(this.$refs.enddate)

    // this.queryBoxes()
  },

  methods: {
    closeDialog() {
      this.$emit('close-Dialog')
    },
    queryBoxes() {},

  },

}
</script>
