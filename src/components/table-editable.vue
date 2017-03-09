<style lang="scss" scoped>
.vm-table-wrap {
  max-height: 500px;
  overflow-y: auto;
}
.vm-table {
  text-align: center;

  td, th {
    text-align: center;
  }

  input {
    width: 100%;
    border: 0;
    outline: 0;
    background-color: transparent;
    text-align: center;
  }

  .vm-checkbox {
    width: 16px;
    height: 16px;
    vertical-align: middle;
  }
}
</style>

<template lang="html">
  <div>
    <div class="vm-table-wrap" style="clear:both;">
      <table class="table table-striped" style="font-size:16px;border:1px solid rgb(102, 102, 102);">
        <thead>
          <tr>
            <th style="width: 50px;" class="control">
              <input type="checkbox" class="checkbox vm-checkbox" v-model="checkboxAll" @change="chooseAll">
            </th>
            <th v-for="title in titles">{{title}}</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(data, index) in data">
            <td class="control"><input type="checkbox" class="checkbox vm-checkbox" :checked="checkboxes[index]" @change="toggleCheckbox($event, index)"></td>
            <td v-if="isEditable" v-for="key in keys" class="control"><input type="text" v-model="data[key]" @focus="focus"></td>
            <td v-if="!isEditable" v-for="key in keys">{{data[key]}}</td>
          </tr>
        </tbody>
        <tfoot v-if="isCalculate && data.length">
          <tr>
            <td>合计</td>
            <td v-if="isCalculate" v-for="key in keys">{{sums[key]}}</td>
          </tr>
        </tfoot>
      </table>
    </div>

    <!-- <div class="vm-vertical-block">
      <button type="button" class="btn btn-default" v-if="isDelete" :class="{'is-disabled': !isLineSelected}" @click="deleteLine">删除行</button>
      <button type="button" class="btn btn-default" v-if="isSelect" :class="{'is-disabled': !isLineSelected}" @click="confirmSelect">选择</button>
      <button type="button" class="btn btn-default" v-if="isSelect" :class="{'is-disabled': !isLineSelected}" @click="addLine">添加行</button>
    </div> -->
  </div>
</template>

<script>

import _ from 'lodash'

export default {
  data() {
    return {
      checkboxAll: false,
      // checkboxes: Array(this.data.length),
    }
  },

  props: {
    data: {
      type: Array,
      required: true,
    },
    titles: {
      type: Array,
      required: true,
    },
    keys: {
      type: Array,
      required: true,
    },
    isCalculate: {
      type: Boolean,
      default: false,
    },
    isEditable: {
      type: Boolean,
      default: false,
    },
    calculableKey: {
      type: Array,
    },
    caculatedPrecision: {
      type: Number,
      default: 0,
    },
    isDelete: {
      type: Boolean,
      default: false,
    },
    isSelect: {
      type: Boolean,
      default: false,
    },
  },

  computed: {
    checkboxes() {
      return _.map(this.data, () => false)
    },

    sums() {
      const result = {}

      // 计算出需要合计的列
      _.each(this.data, (item) => {
        _.each(item, (value, key) => {
          if (_.indexOf(this.keys, key) >= 0) {
            if (result[key] === undefined) {
              result[key] = _.indexOf(this.calculableKey, key) >= 0 ? 0 : '--'
            }

            if (_.indexOf(this.calculableKey, key) >= 0) {
              result[key] += Number(value)
            }
          }
        })
      })

      // 设置计算精度
      _.each(result, (value, key) => {
        if (typeof value === 'number') {
          result[key] = value.toFixed(this.caculatedPrecision)
        }
      })

      return result
    },

    isLineSelected() {
      return this.checkboxes.some(item => item)
    },
  },

  methods: {
    focus(event) {
      event.target.select()
    },

    chooseAll() {
      const isAllChecked = _.every(this.checkboxes, item => item === true)

      if (isAllChecked) {
        _.each(this.checkboxes, (item, index) => {
          this.$set(this.checkboxes, index, false)
        })
      } else {
        _.each(this.checkboxes, (item, index) => {
          this.$set(this.checkboxes, index, true)
        })
      }
    },

    deleteLine() {
      const indexes = []

      _.eachRight(this.checkboxes, (item, index) => {
        if (item === true) {
          // this.data.splice(index, 1)
          indexes.push(index)
          // this.checkboxes.splice(index, 1)
        }
      })

      this.checkboxAll = false

      this.$emit('delete-line', indexes)
    },

    addLine() {
      this.$emit('add-line')
    },

    confirmSelect() {
      const selectedIndexes = []

      _.each(this.checkboxes, (item, index) => {
        if (item) {
          selectedIndexes.push(index)
        }
      })

      this.$emit('confirm-select', selectedIndexes)
    },

    toggleCheckbox(event, index) {
      this.$set(this.checkboxes, index, event.target.checked)
    },
  },
}
</script>
