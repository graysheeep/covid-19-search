<template>
  <div class="container">
    <h1>Covid-19 查询感染记录</h1>
    <form>
      <div class="form-group">
        <label for="exampleInputEmail1">Your Address</label>
        <input type="text" class="form-control" id="searchText" value="keyword" v-model="keyword">
        <small id="emailHelp" class="form-text text-muted">We'll never share your address with anyone else.</small>
      </div>
      <button type="submit" class="btn btn-primary btn-block" @click.stop.prevent="onSearch">Search</button>
    </form>
  </div>
</template>

<script>
const cheerio = require('cheerio')
export default {
  name: 'IndexPage',
  async asyncData({isDev, route, store, env, params, query, req, res, redirect, error, $axios}) {
    const resp = await $axios.get('https://chenfan.info/iframe_search/%E4%B8%AD%E5%B1%B1%E5%8D%97%E8%B7%AF1358%E5%BC%84')
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
      window.location.href = `/search?keyword=${encodeURIComponent(encodeURIComponent(this.keyword))}`
      // this.$router.push({ path: '/search?keyword=' + this.keyword })
    }
  }
}
</script>

<style>
.container {
  padding: 20px;
}

.container h1 {
  font-size: 24px;
  text-align: center;
  color: #555;
  margin-bottom: 50px;
}

.container form {
  /* margin-top: 30%; */
}
</style>