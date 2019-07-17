<template>
  <div class="select-ab">
    <el-select
      v-model="value"
      clearable
      filterable
      @change="changeMethod"
      remote
      reserve-keyword
      placeholder="请输入关键词"
      :remote-method="remoteMethod"
      :loading="loading">
      <el-option
        v-for="item in options"
        :key="item.value"
        :label="item.label"
        :value="item.value">
      </el-option>
    </el-select>
  </div>
</template>

<script>
export default {
  name: 'select-ab',
  props: ['statesA', 'statesB'],
  data () {
    return {
      options: [],
      value: [],
      list: [],
      loading: false
    }
  },
  mounted () {
    this.jiekouA()
  },
  methods: {
    jiekouA () {
      console.log('调用A')
      this.list = this.statesA.map(item => {
        return { value: item, label: item }
      })
      this.options = this.list
    },
    jiekouB (query) {
      console.log('调用B')
      this.list = this.statesB.map(item => {
        return { value: item, label: item }
      })
      this.remoteMethod(query, true)
    },
    changeMethod (value) {
      console.log('value', value)
      this.$emit('changeMethod', value)
      this.remoteMethod(value)
    },
    remoteMethod (query, flag) {
      console.log('query', query)
      if (query !== '') {
        this.loading = true
        setTimeout(() => {
          this.loading = false
          this.options = this.list.filter(item => {
            let you = item.label.toLowerCase().indexOf(query.toLowerCase()) > -1
            // console.log('you', you)
            return you
          })
          if (this.options.length === 0 && !flag) {
            this.jiekouB(query)
          }
        }, 200)
      } else {
        this.jiekouA()
      }
    }
  }
}
</script>
