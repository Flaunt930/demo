<template>
  <div class="demo">
    <!--<select-ab-->
      <!--:statesA="statesA"-->
      <!--:statesB="statesB"-->
      <!--@changeMethod="changeMethod">-->
    <!--</select-ab>-->
    <input-ab
      ref="inputAB"
      :statesA="statesC"
      :statesB="statesD"
      @changeMethod="changeMethod">
    </input-ab>

    <el-row>
      <el-button plain size="mini" @click="dialogDelete = true">删除</el-button>
      <el-button plain size="mini" @click="dialogVisible = true">筛选</el-button>
      <el-button plain size="mini" @click="resetClick">重置</el-button>
      <el-button plain size="mini" @click="selectValueClick">取值</el-button>
    </el-row>
    <el-table
      ref="multipleTable"
      :data="tableData"
      style="width:100%"
      @selection-change="handleSelectionChange">
      <el-table-column type="selection" width="55"></el-table-column>
      <el-table-column type="index" width="55" label="序号"></el-table-column>
      <el-table-column
        v-for="(item,index) in allOption"
        v-if="item.show"
        :key="'tab'+index"
        :prop="item.prop"
        :label="item.label">
      </el-table-column>
    </el-table>
    <el-dialog title="筛选" :visible.sync="dialogVisible" width="60%">
      <el-checkbox
        :indeterminate="isIndeterminate"
        v-model="checkAll"
        @change="handleCheckAllChange">
        全选
      </el-checkbox>
      <div style="margin: 15px 0;"></div>
      <el-checkbox-group
        v-model="checkedCities"
        @change="handleCheckedCitiesChange">
        <el-checkbox v-for="(item,index) in allOption"
                     :key="'all'+index"
                     :disabled="item.disabled"
                     :label="item.prop">
          {{item.label}}
        </el-checkbox>
      </el-checkbox-group>
      <el-row style="margin-top:20px;">
        <el-button plain size="mini" @click="dialogVisible = false">取 消</el-button>
        <el-button plain size="mini" @click="sureClick">确 定</el-button>
      </el-row>
    </el-dialog>

    <!--删除-->
    <el-dialog
      title="提示"
      :visible.sync="dialogDelete"
      width="400px">
      <span>确定要删除第{{row_index}}行吗？</span>
      <el-row>
        <el-button @click="dialogDelete = false">取 消</el-button>
        <el-button type="primary" @click="deleteClick">确 定</el-button>
      </el-row>
    </el-dialog>
  </div>
</template>

<script>
import selectAb from './select-ab.vue'
import inputAb from './input-ab.vue'

export default {
  name: 'demo',
  components: {
    selectAb, inputAb
  },
  data () {
    return {
      statesA: ['Alabama', 'Alaska', 'Arizona',
        'Arkansas', 'California', 'Colorado',
        'Connecticut', 'Delaware', 'Florida',
        'Georgia', 'Hawaii', 'Idaho', 'Illinois',
        'Indiana', 'Iowa', 'Kansas', 'Kentucky',
        'Louisiana', 'Maine', 'Maryland'],
      statesB: [
        'Massachusetts', 'Michigan', 'Minnesota',
        'Mississippi', 'Missouri', 'Montana',
        'Nebraska', 'Nevada', 'New Hampshire',
        'New Jersey', 'New Mexico', 'New York',
        'North Carolina', 'North Dakota', 'Ohio',
        'Oklahoma', 'Oregon', 'Pennsylvania',
        'Rhode Island', 'South Carolina',
        'South Dakota', 'Tennessee', 'Texas',
        'Utah', 'Vermont', 'Virginia',
        'Washington', 'West Virginia', 'Wisconsin',
        'Wyoming'],
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
      queryValue: '',
      allOption: [
        {prop: 'date', label: '日期', disabled: true, show: true},
        {prop: 'name', label: '名字', disabled: true, show: true},
        {prop: 'address', label: '地址', disabled: false, show: true},
        {prop: 'province', label: '省份', disabled: false, show: true},
        {prop: 'city', label: '城市', disabled: false, show: false},
        {prop: 'category', label: '类型', disabled: false, show: false},
        {prop: 'desc', label: '描述', disabled: true, show: false}
      ],
      tableData: [
        {
          date: '2016-05-03',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄',
          province: '上海',
          city: '普陀区',
          category: '江浙小吃、小吃零食',
          desc: '荷兰优质淡奶，奶香浓而不腻'
        },
        {
          date: '2016-05-03',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄',
          province: '上海',
          city: '普陀区',
          category: '江浙小吃、小吃零食',
          desc: '荷兰优质淡奶，奶香浓而不腻'
        },
        {
          date: '2016-05-03',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄',
          province: '上海',
          city: '普陀区',
          category: '江浙小吃、小吃零食',
          desc: '荷兰优质淡奶，奶香浓而不腻'
        },
        {
          date: '2016-05-03',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄',
          province: '上海',
          city: '普陀区',
          category: '江浙小吃、小吃零食',
          desc: '荷兰优质淡奶，奶香浓而不腻'
        },
        {
          date: '2016-05-03',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄',
          province: '上海',
          city: '普陀区',
          category: '江浙小吃、小吃零食',
          desc: '荷兰优质淡奶，奶香浓而不腻'
        },
        {
          date: '2016-05-03',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄',
          province: '上海',
          city: '普陀区',
          category: '江浙小吃、小吃零食',
          desc: '荷兰优质淡奶，奶香浓而不腻'
        }
      ],
      dialogVisible: false,
      dialogDelete: false,
      row_index: '',
      checked: false,
      checkAll: false,
      checkedCities: ['date', 'name', 'address', 'province'],
      defaultSelectValue: ['date', 'name', 'address', 'province'],
      allSelectValue: ['date', 'name', 'address', 'province', 'city', 'category'],
      isIndeterminate: true,
      formInfo: {
        id: 'B8',
        prop: 'desc',
        label: '描述',
        show: false,
        size: '50'
      },
      index: 0
    }
  },
  mounted () {
    this.formatter()
    this.pamarsFormat()
  },
  methods: {
    pamarsFormat () {
      let isTwo = ['prop', 'label']
      let isThree = ['size', 'show']
      let aaa = {
        a: {id: ''},
        b: [],
        c: []
      }
      for (let key in this.formInfo) {
        if (key === 'id') {
          aaa.a.id = this.formInfo.id
        } else if (isTwo.some((item) => { return item === key })) {
          let two = {key: key, value: this.formInfo[key]}
          aaa.b.push(two)
        } else if (isThree.some((item) => { return item === key })) {
          let three = {key: key, value: this.formInfo[key]}
          aaa.c.push(three)
        }
      }
      aaa = JSON.parse(JSON.stringify(aaa))
      console.log(aaa)
      let pamars = {
        first: {id: 'B8'},
        two: [
          {key: 'prop', value: 'desc'},
          {key: 'label', value: '描述'}
        ],
        three: [
          {key: 'size', value: '50'},
          {key: 'show', value: 'false'}
        ]
      }
      console.log(pamars)
    },
    formatter () {
      let data = [
        '三全鲜食（北新泾店）', 'Hot honey 首尔炸鸡（仙霞路）', 'Hot honey 首尔炸鸡（仙霞路）',
        '新旺角茶餐厅', '泷千家(天山西路店)', '贡茶', '胖仙女纸杯蛋糕（上海凌空店）', '豪大大香鸡排超级奶爸',
        '茶芝兰（奶茶，手抓饼）', '阿姨奶茶/豪大大'
      ]
      data = data || []
      let list = data.map(item => {
        return { value: item }
      })
      this.statesD = list
      // sconsole.log(this.statesD)
    },
    handleCheckAllChange (val) {
      this.checkedCities = val ? this.allSelectValue : this.defaultSelectValue
      this.isIndeterminate = false
    },
    handleCheckedCitiesChange (value) {
      let checkedCount = value.length
      this.checkAll = checkedCount === this.allSelectValue.length
      this.isIndeterminate = checkedCount > 0 && checkedCount < this.allSelectValue.length
    },
    changeMethod (value) {
      this.queryValue = value
      console.log('this.queryValue ', this.queryValue)
    },
    sureClick () {
      this.allOption.map(item => {
        let hasData = this.checkedCities.some(function (value) {
          return value === item.prop
        })
        if (hasData) {
          item.show = true
        } else {
          item.show = false
        }
      })
      this.dialogVisible = false
    },
    handleSelectionChange (val) {
      console.log(val)
    },
    deleteClick () {
      this.dialogDelete = false
    },
    resetClick () {
      this.$refs.inputAB.clear()
    },
    selectValueClick () {
      let data = ['first', 'two', 'three', 'for']
      let selectValue = data[this.index]
      this.index++
      if (this.index === data.length) this.index = 0
      console.log(selectValue)
    }
  }
}
</script>

<style>
  .el-table th.el-table-column--selection .el-checkbox {
    display: none;
  }
</style>
