<template>
  <h1>App</h1>
  <hr>
  <MyTable :data="goodslist">
  <template v-slot:header>
    <th>#</th>
    <th>商品名稱</th>
    <th>價格</th>
    <th>標籤</th>
    <th>操作</th>
  </template>
  <template v-slot:body="{row, index}">
    <td>{{ index + 1 }}</td>
    <td>{{ row.goods_name }}</td>
    <td>${{ row.goods_price }}</td>
    <td>
      <input type="text" class="form-control form-control-sm form-ipt" v-if="row.inputVisible" v-focus v-model.trim="row.inputValue" @blur="onInputConfirm(row)" @keyup.enter="onInputConfirm(row)" @keyup.esc="row.inputValue = ''"/>
      <button type="button" class="btn btn-primary" v-else @click="row.inputVisible = true">+Tag</button>
      <!-- 標籤 -->
      <span class="badge text-bg-warning ms-2" v-for="item in row.tags" :key="item">{{item}}</span>
    </td>
    <td>
      <button type="button" class="btn btn-danger btn-sm" @click="onRemove(row.id)">刪除</button>
    </td>
  </template>
  </MyTable>
</template>

<script>
  import MyTable from './components/my-table/MyTable.vue'

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
      },
      //根據id刪除商品
      onRemove(id) {
        this.goodslist = this.goodslist.filter(x => x.id !== id)
      },
      onInputConfirm(row) {
        const val = row.inputValue
        row.inputValue = ''
        row.inputVisible = false
        if(!val || row.tags.indexOf(val) !== -1) return
        row.tags.push(val)
      }
    },
    directives: {
      focus(el) {
        el.focus()
      }
    },
    components: {
      MyTable
    }
  }
</script>

<style lang="less" scoped>
.form-ipt {
  width: 80px;
  display: inline;
}
</style>