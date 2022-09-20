<template>
  <div>App</div>
  <hr>
</template>

<script>
  export default {
    name: "MyApp",
    data() {
      return {
        goodslist:[]
      }
    },
    created(){
      //發起get
      this.getGoodsList()
    },
    methods: {
      async getGoodsList() {
        const {data: res} = await this.$http.get('/api/goods')
        if(res.status !== 0) return console.log('getGoodsList FAILED')
        this.goodslist = res.data
      }
    }
  }
</script>

<style lang="less" scoped>

</style>