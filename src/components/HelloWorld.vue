<template>
  <div class="demo">
    <div style="margin-bottom: 20px">
      <input-ab
        :label="'man'"
        :prop="man"
        :statesA="statesC"
        :statesB="statesD"
        @changeMethod="Method">
      </input-ab>

      <input-ab
        :label="'wuman'"
        :prop="wuman"
        :statesA="statesC"
        :statesB="statesD"
        @changeMethod="Method">
      </input-ab>
    </div>
    <el-table
      :data="tableData6"
      border
      style="width: 100%; margin-top: 20px">
      <el-table-column prop="name" label="姓名">
        <template slot-scope="scope">
          <input-ab
            :index="scope.$index"
            :prop="scope.row.name"
            :statesA="statesC"
            :statesB="statesD"
            @changeMethod="changeMethod">
          </input-ab>
        </template>
      </el-table-column>
      <el-table-column prop="amount1" label="数值 1（元）"></el-table-column>
      <el-table-column prop="amount2" label="占比（%）"></el-table-column>
      <el-table-column prop="amount3" label="数值 3（元）"></el-table-column>
      <el-table-column prop="amount4" label="占比（%）"></el-table-column>
    </el-table>
  </div>
</template>

<script>
import inputAb from './input-ab.vue'

export default {
  components: {
    inputAb
  },
  data () {
    return {
      man: '',
      wuman: '',
      chnNumChar: ['一', '二', '三', '四', '五', '六', '七', '八'],
      statesC: [
        { 'value': '新麦甜四季甜品炸鸡', 'address': '嘉定区曹安公路2383弄55号' },
        { 'value': 'Monica摩托主题咖啡店', 'address': '嘉定区江桥镇曹安公路2409号1F，2383弄62号1F' },
        { 'value': '浮生若茶（凌空soho店）', 'address': '上海长宁区金钟路968号9号楼地下一层' },
        { 'value': 'NONO JUICE  鲜榨果汁', 'address': '上海市长宁区天山西路119号' },
        { 'value': 'CoCo都可(北新泾店）', 'address': '上海市长宁区仙霞西路' },
        { 'value': '快乐柠檬（神州智慧店）', 'address': '上海市长宁区天山西路567号1层R117号店铺' },
        { 'value': 'Merci Paul cafe', 'address': '上海市普陀区光复西路丹巴路28弄6号楼819' },
        { 'value': '猫山王（西郊百联店）', 'address': '上海市长宁区仙霞西路88号第一层G05-F01-1-306' },
        { 'value': '枪会山', 'address': '上海市普陀区棕榈路' },
        { 'value': '纵食', 'address': '元丰天山花园(东门) 双流路267号' },
        { 'value': '钱记', 'address': '上海市长宁区天山西路' },
        { 'value': '壹杯加', 'address': '上海市长宁区通协路' },
        { 'value': '唦哇嘀咖', 'address': '上海市长宁区新泾镇金钟路999号2幢（B幢）第01层第1-02A单元' }
      ],
      statesD: [],
      tableData6: [
        {name: '王', amount1: '39', amount2: '4.1%', amount3: 15, amount4: '4.12%'},
        {name: '王小', amount1: '79', amount2: '5.1%', amount3: 16, amount4: '4.11%'},
        {name: '小虎', amount1: '59', amount2: '6.1%', amount3: 17, amount4: '14.14%'},
        {name: '王小虎', amount1: '939', amount2: '7.1%', amount3: 18, amount4: '42.11%'}
      ]
    }
  },
  methods: {
    Method (val, label) {
      this[label] = val
      console.log(this.man)
      console.log(this.wuman)
    },
    changeMethod (value, index) {
      console.log(value, index)
      this.tableData6[index].name = value
      console.log(this.tableData6)
    }
  }
}
</script>

<style scoped>
  .demo {
    padding: 15px 20px;
  }

  .form {
    display: flex;
    margin-top: 20px;
  }

  .firstCloumn {
    width: 80px;
    text-align: right;
    padding-top: 27px;
  }

  .firstCloumn div {
    height: 30px;
    line-height: 30px;
    margin-bottom: 10px;
  }

  .plusBtn {
    text-align: center;
  }

  .roundness {
    background-color: #888;
    border-radius: 20px;
    color: #fff;
    display: inline-block;
    font-size: 14px;
    height: 19px;
    line-height: 18px;
    padding: 0 6px;
    text-align: center;
    white-space: nowrap;
    border: 1px solid #888;
    margin-left: 15px;
  }

  .plus {
    padding: 0 5px;
  }

  .el-form-item {
    margin-bottom: 0;
  }

  .plusBtn .el-button + .el-button {
    margin-left: 0px;
  }

  .item {
    margin-right: 10px;
  }

  .el-form {
    display: flex;
  }

  .firstCloumn div:before {
    content: "*";
    color: #f56c6c;
    margin-right: 4px;
  }

  .formCloumn .el-form .el-form-item__error {
    opacity: 0;
  }
</style>
