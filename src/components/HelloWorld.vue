<template>
  <div class="demo">
    <el-button @click="submitClick('formInfo')" size="mini" plain>提交</el-button>
    <div class="form">
      <div class="firstCloumn">
        <div>星座：</div>
        <div>想法：</div>
        <div>优势：</div>
      </div>
      <div class="formCloumn">
        <el-form :model="formInfo" ref="formInfo">
          <div class="item" v-for="(item, index) in formInfo.formData" :key="'data'+index">
            <div class="plusBtn">方案{{chnNumChar[index]}}
              <el-button :disabled="formInfo.formData.length==8" class="roundness plus" @click="cloumnPlus">+</el-button>
              <el-button :disabled="formInfo.formData.length==1" class="roundness" @click="deleteClick(index)">-</el-button>
            </div>
            <el-form-item :rules="[{ required: true, trigger: 'change' }]" prop="constellation">
              <el-input size="mini" v-model="item.constellation"></el-input>
            </el-form-item>
            <el-form-item :rules="[{ required: true, trigger: 'change' }]" prop="idea">
              <el-input size="mini" v-model="item.idea"></el-input>
            </el-form-item>
            <el-form-item :rules="[{ required: true, trigger: 'change' }]" prop="advantage">
              <el-input size="mini" v-model="item.advantage"></el-input>
            </el-form-item>
          </div>
        </el-form>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      chnNumChar: ['一', '二', '三', '四', '五', '六', '七', '八'],
      formInfo: {
        formData: [
          {id: 1, constellation: '', idea: '', advantage: ''}
        ]
      }
    }
  },
  methods: {
    submitClick (formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          this.$message.success('提交成功')
        } else {
          this.$message.error('请输入信息')
        }
      })
    },
    cloumnPlus () {
      let obj = {constellation: '', idea: '', advantage: ''}
      this.formInfo.formData.push(obj)
    },
    deleteClick (index) {
      this.formInfo.formData.splice(index, 1)
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
    width:80px;
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
   margin-left:15px;
 }
  .plus {
    padding: 0 5px;
  }
  .el-form-item {
    margin-bottom: 0;
  }
 .plusBtn .el-button+.el-button {
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
    opacity: 0 ;
  }
</style>
