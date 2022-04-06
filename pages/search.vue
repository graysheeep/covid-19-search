<template>
  <div class="container">
    <h3>查询结果</h3>
    <table class="table table-striped">
      <thead class="thead-dark">
        <tr>
          <th scope="col">#</th>
          <th scope="col">区域</th>
          <th scope="col">地址</th>
          <th scope="col">上报日期</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(item, index) in data.slice(2)" :key="index">
          <th scope="row">{{ index + 1 }}</th>
          <td>{{ data[0] }}</td>
          <td>{{ data[1] }}</td>
          <td>{{ item }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
const cheerio = require('cheerio')
export default {
  name: 'IndexPage',
  async asyncData({isDev, route, store, env, params, query, req, res, redirect, error, $axios}) {
    // console.log({ params, query , req })
    const { keyword = '' } = query
    console.log({ keyword })
    const resp = await $axios.get(`https://chenfan.info/iframe_search/${keyword}`)
    const $ =  cheerio.load(resp.data)
    const dateList = []
    $('tbody th').each((index, item) => {
      dateList.push($(item).text())
    })
    // const data = await resp.text()
    // console.log({ resp })
    return {
      keyword: '',
      data: dateList
    }
  },
  methods: {
    onSearch() {
      alert('search')
    }
  }
}
</script>

<style>
.container {
  padding: 20px;
}

h3 {
  color: #555;
  margin-bottom: 30px;
  font-size: 20px;
  text-align: center;
}
</style>