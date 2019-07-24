<template>
  <div class="input-ab">
    <el-autocomplete
      v-model="state4"
      clearable
      :fetch-suggestions="querySearchAsync"
      placeholder="请选择或输入内容"
      @select="handleSelect"
    ></el-autocomplete>
  </div>
</template>

<script>
export default {
  name: 'input-ab',
  props: ['statesA', 'statesB'],
  data () {
    return {
      restaurants: [],
      state4: '',
      timeout: null
    }
  },
  mounted () {
    this.jiekouA()
  },
  methods: {
    jiekouA () {
      this.restaurants = this.statesA
    },
    jiekouB () {
      this.restaurants = this.statesB
      // console.log(this.restaurants)
    },
    querySearchAsync (queryString, cb) {
      let hasState = this.createStateFilter(queryString)
      let restaurants = this.restaurants
      let results = queryString ? restaurants.filter(hasState) : restaurants
      // console.log('results', results)
      clearTimeout(this.timeout)
      this.timeout = setTimeout(() => {
        cb(results)
      }, 500)
      this.$emit('changeMethod', this.state4)
    },
    createStateFilter (queryString) {
      return (state) => {
        let hasState = state.value.toLowerCase().indexOf(queryString.toLowerCase()) === 0
        // console.log('hasState', hasState)
        if (!hasState) this.jiekouB()
        return hasState
      }
    },
    handleSelect (item) {
      this.$emit('changeMethod', this.state4)
    },
    clear () {
      this.state4 = ''
    }
  }
}
</script>
