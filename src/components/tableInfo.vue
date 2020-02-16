<template>
    <div class="tableDemo">
        <el-form ref="form" :model="form">
            <el-table
                    :data="form.tableData"
                    style="width: 100%"
                    border size="mini"
                    highlight-current-row
                    :header-cell-style="{background:'#f5f7fa'}"
                    :row-class-name="tableRowClassName"
                    @cell-click="cellClick">
                <el-table-column type="index" label="序号" width="80px"></el-table-column>
                <el-table-column prop="text" label="值(中文)" min-width="180px">
                    <template slot-scope="scope">
                        <el-form-item v-if="scope.row.index === tabClickIndex && tabClickLabel === '值(中文)'">
                            <el-input size="mini" v-model="scope.row.text" placeholder="请输入"></el-input>
                        </el-form-item>
                        <span v-else>{{scope.row.text}}</span>
                    </template>
                </el-table-column>
                <el-table-column prop="value" label="值(English)" min-width="180px">
                    <template slot-scope="scope">
                        <el-form-item v-if="scope.row.index === tabClickIndex && tabClickLabel === '值(English)'">
                            <el-input size="mini" v-model="scope.row.value" placeholder="请输入"></el-input>
                        </el-form-item>
                        <span v-else>{{scope.row.value}}</span>
                    </template>
                </el-table-column>
                <el-table-column label="操作" width="240px">
                    <template slot-scope="scope">
                        <el-button type="primary" icon="el-icon-plus" size="mini" @click="plusRow(form.tableData, scope.$index)"></el-button>
                        <el-button type="primary" icon="el-icon-minus" size="mini" @click="deleteRow(form.tableData, scope.$index)"></el-button>
                        <el-button type="primary" icon="el-icon-upload2" size="mini" @click="moveUpRow(form.tableData, scope.$index)"></el-button>
                        <el-button type="primary" icon="el-icon-download" size="mini" @click="moveDownRow(form.tableData, scope.$index)"></el-button>
                    </template>
                </el-table-column>
            </el-table>
        </el-form>
        <div class="mb20">
            <el-button type="primary" plain size="mini" @click="formatData">arry-obj</el-button>
            <el-button type="primary" plain size="mini" @click="formatData2">obj-arry</el-button>
            <el-button type="primary" plain size="mini" @click="formatData3">obj-arry-2</el-button>
        </div>
    </div>
</template>

<script>
export default {
    name: "tableDemo",
    data(){
        return{
            form:{
                tableData: [
                    {text: '尺寸', value: 'size'},
                    {text: '客户', value: 'client'},
                    {text: '分辨率', value: 'resolution'},
                ]
            },
            tabClickIndex: null,  // 点击的单元格
            tabClickLabel: '',   // 当前点击的列名
            dateObj: {
                BA_date: {key: 'BA_date', value: '2020-01-01', canInsert: false},
                PP_plan_date: {key: 'PP_plan_date', value: '2020-02-01', canInsert: false},
                BOE_Plan_date: {key: 'BOE_Plan_date', value: '2020-01-20', canInsert: false},
                PP_on_date: {key: 'PP_on_date', value: '2020-02-10', canInsert: false},
                MP_plan_date: {key: 'MP_plan_date', value: '2020-01-15', canInsert: false}
            }
        }
    },
    methods: {
        tableRowClassName({ row, rowIndex }){
            row.index = rowIndex
        },
        cellClick(row, column){
            let label = ['值(中文)', '值(English)']
            label.map(item=>{
                if(column.label === item){
                    this.tabClickIndex = row.index
                    this.tabClickLabel = column.label
                }
            })
        },
        plusRow(rows, index){
            let item = {text: '', value: ''}
            rows.splice((index+1), 0, item)
        },
        deleteRow(rows, index){
           rows.splice(index, 1)
        },
        // 上移
        moveUpRow(rows, index){
            if(index>0){
                let upData = rows[index - 1];
                rows.splice(index - 1, 1)
                rows.splice(index, 0, upData)
            }else {
                this.$message.error('已经是第一条，不可上移')
            }
        },
        // 下移
        moveDownRow(rows, index){
            if((index + 1) === rows.length){
                this.$message.error('已经是最后一条，不可下移')
            }else {
                let downData = rows[index + 1];
                rows.splice(index + 1, 1)
                rows.splice(index, 0, downData)
            }
        },
        // 数据格式  ---  数组拼接列表对象格式
        formatData(){
            let data = [
                ['BYD', 0, 2, 1, 3],
                ['丰艺', 0, 2, 1, 3],
                ['二线', 4, 3, 5, 12]
            ],list = []
            data.map(item=>{
                let obj = { client: item[0], data1: item[1], data2: item[2], data3: item[3], total: item[4]}
                list.push(obj)
            })
            this.$message({
                message: list,
                type: 'success'
            })
        },
        // 数据格式  ---  对象转数组
        formatData2(){
            let data = {
                BA_date: {key: 'BA_date', value: '2020-01-01', canInsert: false},
                PP_plan_date: {key: 'PP_plan_date', value: '2020-02-01', canInsert: false},
                BOE_Plan_date: {key: 'BOE_Plan_date', value: '2020-01-20', canInsert: false},
                PP_on_date: {key: 'PP_on_date', value: '2020-02-10', canInsert: false},
                MP_plan_date: {key: 'MP_plan_date', value: '2020-01-15', canInsert: false}
            },
            isKey = ['BA_date', 'PP_plan_date', 'BOE_Plan_date', 'PP_on_date', 'MP_plan_date'],
            arry = []
            for(let key in data){
                if(isKey.some((item)=>{return item === key})){
                    let obj = {key: key, value: data[key].value}
                    arry.push(obj)
                }
            }
            this.$message({
                message: arry,
                type: 'success'
            })
        },
        // 数据格式  ---  对象转数组
        formatData3(){
            let data = this.convertFormJson(this.dateObj)
            this.$message({
                message: data,
                type: 'success'
            })
        },
        convertFormJson(json){
            let convertJson = []
            let jsonKeys = Object.keys(json)
            jsonKeys.forEach((value)=>{
                convertJson.push({
                    'key': value,
                    'value': json[value].value
                })
            })
            return convertJson
        }
    }
}
</script>

<style>
.tableDemo {
    text-align: left;
}
.tableDemo .mb20 {
    margin-top: 20px;
}
.tableDemo .el-table td,
.tableDemo .el-table th {
    text-align: center;
}
.tableDemo .el-form-item {
    margin-bottom: 0px;
}
.tableDemo .el-table .el-button--mini, .el-button--mini.is-round {
    padding: 8px;
}
</style>
