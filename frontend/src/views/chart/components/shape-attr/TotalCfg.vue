<template>
  <div style="width: 100%">
    <el-col>
      <el-form ref="totalForm" :model="totalForm" label-width="80px" size="mini">
        <el-divider content-position="center" class="divider-style">{{ $t('chart.row_cfg') }}</el-divider>
        <el-form-item :label="$t('chart.total_show')" class="form-item">
          <el-checkbox v-model="totalForm.row.showGrandTotals" @change="changeTotalCfg">{{ $t('chart.show') }}</el-checkbox>
        </el-form-item>
        <div v-show="totalForm.row.showGrandTotals">
          <el-form-item :label="$t('chart.total_position')" class="form-item">
            <el-radio-group v-model="totalForm.row.reverseLayout" @change="changeTotalCfg">
              <el-radio :label="true">{{ $t('chart.total_pos_top') }}</el-radio>
              <el-radio :label="false">{{ $t('chart.total_pos_bottom') }}</el-radio>
            </el-radio-group>
          </el-form-item>
          <el-form-item :label="$t('chart.total_label')" class="form-item">
            <el-input v-model="totalForm.row.label" style="width: 160px;" :placeholder="$t('chart.total_label')" size="mini" clearable @change="changeTotalCfg" />
          </el-form-item>
        </div>

        <el-form-item :label="$t('chart.sub_total_show')" class="form-item">
          <el-checkbox v-model="totalForm.row.showSubTotals" :disabled="rowNum < 2" @change="changeTotalCfg">{{ $t('chart.show') }}</el-checkbox>
        </el-form-item>
        <div v-show="totalForm.row.showSubTotals">
          <el-form-item :label="$t('chart.total_position')" class="form-item">
            <el-radio-group v-model="totalForm.row.reverseSubLayout" :disabled="rowNum < 2" @change="changeTotalCfg">
              <el-radio :label="true">{{ $t('chart.total_pos_top') }}</el-radio>
              <el-radio :label="false">{{ $t('chart.total_pos_bottom') }}</el-radio>
            </el-radio-group>
          </el-form-item>
          <el-form-item :label="$t('chart.total_label')" class="form-item">
            <el-input v-model="totalForm.row.subLabel" :disabled="rowNum < 2" style="width: 160px;" :placeholder="$t('chart.total_label')" size="mini" clearable @change="changeTotalCfg" />
          </el-form-item>
        </div>

        <el-divider content-position="center" class="divider-style">{{ $t('chart.col_cfg') }}</el-divider>
        <el-form-item :label="$t('chart.total_show')" class="form-item">
          <el-checkbox v-model="totalForm.col.showGrandTotals" @change="changeTotalCfg">{{ $t('chart.show') }}</el-checkbox>
        </el-form-item>
        <div v-show="totalForm.col.showGrandTotals">
          <el-form-item :label="$t('chart.total_position')" class="form-item">
            <el-radio-group v-model="totalForm.col.reverseLayout" @change="changeTotalCfg">
              <el-radio :label="true">{{ $t('chart.total_pos_left') }}</el-radio>
              <el-radio :label="false">{{ $t('chart.total_pos_right') }}</el-radio>
            </el-radio-group>
          </el-form-item>
          <el-form-item :label="$t('chart.total_label')" class="form-item">
            <el-input v-model="totalForm.col.label" style="width: 160px;" :placeholder="$t('chart.total_label')" size="mini" clearable @change="changeTotalCfg" />
          </el-form-item>
        </div>

        <el-form-item :label="$t('chart.sub_total_show')" class="form-item">
          <el-checkbox v-model="totalForm.col.showSubTotals" :disabled="colNum < 2" @change="changeTotalCfg">{{ $t('chart.show') }}</el-checkbox>
        </el-form-item>
        <div v-show="totalForm.col.showSubTotals">
          <el-form-item :label="$t('chart.total_position')" class="form-item">
            <el-radio-group v-model="totalForm.col.reverseSubLayout" :disabled="colNum < 2" @change="changeTotalCfg">
              <el-radio :label="true">{{ $t('chart.total_pos_left') }}</el-radio>
              <el-radio :label="false">{{ $t('chart.total_pos_right') }}</el-radio>
            </el-radio-group>
          </el-form-item>
          <el-form-item :label="$t('chart.total_label')" class="form-item">
            <el-input v-model="totalForm.col.subLabel" :disabled="colNum < 2" style="width: 160px;" :placeholder="$t('chart.total_label')" size="mini" clearable @change="changeTotalCfg" />
          </el-form-item>
        </div>
      </el-form>
    </el-col>
  </div>
</template>

<script>
import { DEFAULT_TOTAL } from '@/views/chart/chart/chart'

export default {
  name: 'TotalCfg',
  props: {
    chart: {
      type: Object,
      required: true
    }
  },
  data() {
    return {
      totalForm: JSON.parse(JSON.stringify(DEFAULT_TOTAL))
    }
  },
  computed: {
    rowNum() {
      const chart = JSON.parse(JSON.stringify(this.chart))
      if (chart.xaxisExt) {
        let arr = null
        if (Object.prototype.toString.call(chart.xaxisExt) === '[object Array]') {
          arr = JSON.parse(JSON.stringify(chart.xaxisExt))
        } else {
          arr = JSON.parse(chart.xaxisExt)
        }
        return arr.length
      }
      return 0
    },
    colNum() {
      const chart = JSON.parse(JSON.stringify(this.chart))
      if (chart.xaxis) {
        let arr = null
        if (Object.prototype.toString.call(chart.xaxis) === '[object Array]') {
          arr = JSON.parse(JSON.stringify(chart.xaxis))
        } else {
          arr = JSON.parse(chart.xaxis)
        }
        return arr.length
      }
      return 0
    }
  },
  watch: {
    'chart': {
      handler: function() {
        this.initData()
      }
    }
  },
  mounted() {
    this.initData()
  },
  methods: {
    initData() {
      const chart = JSON.parse(JSON.stringify(this.chart))
      if (chart.customAttr) {
        let customAttr = null
        if (Object.prototype.toString.call(chart.customAttr) === '[object Object]') {
          customAttr = JSON.parse(JSON.stringify(chart.customAttr))
        } else {
          customAttr = JSON.parse(chart.customAttr)
        }
        if (customAttr.totalCfg) {
          this.totalForm = customAttr.totalCfg
        } else {
          this.totalForm = JSON.parse(JSON.stringify(DEFAULT_TOTAL))
        }
      }
    },
    changeTotalCfg() {
      this.$emit('onTotalCfgChange', this.totalForm)
    }
  }
}
</script>

<style scoped>
.shape-item{
  padding: 6px;
  border: none;
  width: 100%;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.form-item-slider>>>.el-form-item__label{
  font-size: 12px;
  line-height: 38px;
}
.form-item>>>.el-form-item__label{
  font-size: 12px;
}
.el-select-dropdown__item{
  padding: 0 20px;
}
span{font-size: 12px}

.el-form-item{
  margin-bottom: 6px;
}
.el-divider--horizontal {
  margin: 10px 0
}
.divider-style>>>.el-divider__text{
  color: #606266;
  font-size: 12px;
  font-weight: 400;
  padding: 0 10px;
}
</style>
