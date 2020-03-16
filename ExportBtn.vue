/**
 * introduce : 导出Excel按钮
 * author    : cp
 * date      : 2019/04/29
 说明：项目中要安装插件 FileSaver和XLSX / npm install --save xlsx file-saver // 老版本
 说明新：npm install -S file-saver xlsx npm install -D script-loader // 安装这两个插件
 两个版本的区别 上一个需要获取页面的上表格ID 第二个直接传表头数据和内容数据就行（但是第二个需要新建一个文件夹excel里面放有两个js文件Blob.js和Export2Excel.js使用如下）
 */
<template>
  <div class="export-btn">
    <span class="icon-wrapper"
          @click="exportExcel">
      <i class="iconfont icon-xz1"></i>
    </span>
  </div>
</template>

<script>
export default {
  name: 'ExportBtn',
  props: {
    tbody_name: { // 表格名称
      type: String,
      default () {
        return 'FIELD'
      }
    },
    tabel_label: { // 表头名称
      type: String,
      default () {
        return 'TITLE'
      }
    },
    thead_data: { // 表头数据
      type: Array,
      default () {
        return []
      }
    },
    tbody_data: { // 表格数据
      type: Array,
      default () {
        return []
      }
    },
    special_field: { // 特殊显示字段
      type: String,
      default () {
        return ''
      }
    },
    elport_name: { // 导出表格名称
      type: String,
      default () {
        return '默认导出表格'
      }
    }
  },
  data () {
    return {
      exportList: [{ bookSn: 1, title: '名称', isbnOrIssn: '这个不知道是啥', callNumber: '123456789', price: '987654321', category: '嘿嘿嘿说明' }]
    }
  },
  methods: {
    exportExcel () {
      require.ensure([], () => {
        const { exportjsontoexcel } = require('../../../../excel/Export2Excel')
        const tHeader = this.thead_data.map(item => { return item[this.tabel_label] })
        const filterVal = this.thead_data.map(item => { return item[this.tbody_name] })
        const list = this.tbody_data
        const data = this.formatJson(filterVal, list)
        exportjsontoexcel(tHeader, data, this.elport_name)
      })
    },
    formatJson (filterVal, jsonData) {
      return jsonData.map(v => filterVal.map(j => v[j]))
    }
  }
}
</script>

<style lang="less" scoped>
.export-btn {
  cursor: pointer;
  .icon-wrapper {
    position: relative;
    .anim {
      position: absolute;
      right: -7px;
      top: -4px;
      display: inline-block;
      width: 30px;
      height: 30px;
      transform: scale(0);
      border-radius: 50%;
      background: #7d72bf;
      opacity: 0.5;
      transform-origin: center center;
      transition: all 0.1s ease;
    }
    &:hover {
      .anim {
        transform: scale(1);
      }
    }
  }
}
</style>
