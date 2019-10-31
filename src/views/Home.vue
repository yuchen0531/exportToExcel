<template>
    <div>
      <button @click="exportdata">export</button>
      <table class="table">
        <thead>
          <tr>
            <th >news_id</th>
            <th >標題 </th>
            <th >時間</th>
            <th >排序</th>
            <th >啟用</th>
          </tr>
        </thead>
        <tbody v-for="(article,index) in  articleLists" :key="index">
          <tr>
            <th scope="row">{{article.news_id}} </th>
            <td>{{article.title}}</td>
            <td>{{article.createdon}}</td>
            <td>{{article.sort_ex}}</td>
            <td>是</td>
          </tr>
        </tbody>
      </table>
    </div>
</template>
<script>
// 宣告名稱且引入Export2Excel
const { exportJsonToExcel } = require('../../src/vender/Export2Excel')
export default {
  name: 'home',
  data () {
    return {
      //  宣告tableData
      articleLists: [
        { news_id: 40, title: 'Dickerson', createdon: '2018', sort_ex: 1 },
        { news_id: 21, title: 'Larsen', createdon: '2019', sort_ex: 2 },
        { news_id: 89, title: 'Geneva', createdon: '2020', sort_ex: 3 },
        { news_id: 38, title: 'Jami', createdon: '2017', sort_ex: 4 }
      ]
    }
  },
  methods: {
    exportdata (event) {
      require.ensure([], () => {
        //  自定義表格名稱
        const tHeader = ['自定義ID', '自定義標題', '自定義']
        //  根據tableData的的Key值列出要匯出的，與上方定義名稱的順序為對應的
        const filterVal = ['news_id', 'title', 'sort_ex']
        //  list改為宣告的tableData陣列名稱
        const list = this.articleLists
        const data = this.formatJson(filterVal, list)
        //  '自定義檔名'為輸出的檔名
        //  exportJsonToExcel要對應上方引入的名稱，以及注意Export2Excel裡exportJsonToExcel名稱是否一樣
        exportJsonToExcel(tHeader, data, '自定義檔名')
      })
    },
    formatJson (filterVal, jsonData) {
      return jsonData.map(v => filterVal.map(j => v[j]))
    }
  }
}
</script>
<style lang="stylus" scoped>
  table
    margin 0px auto
    border 1px solid black
    padding 20px

</style>
