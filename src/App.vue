<template>
  <div id="app">
    <v-header :seller="seller"></v-header>
    <v-tab></v-tab>
    <keep-alive>
      <router-view :seller="seller"></router-view>
    </keep-alive>
  </div>
</template>

<script>
  import {urlParse} from './common/js/util.js'
  import header from './components/header/header'
  import tab from './components/tab/tab'

  const ERR_OK = 0

  export default {
    name: 'app',
    data() {
      return {
        seller: {
          id: (() => {
            let queryParam = urlParse()
            return queryParam.id
          })()
        }
      }
    },
    created() {
      // 在url里加入参数id可以区分不同商家的数据，返回不同的seller数据
      this.$http.get('/api/seller?id=' + this.seller.id).then((response) => {
        response = response.body
        if(response.errno === ERR_OK) {
          // 给this.seller扩展属性，保留id属性
          // 第一个参数为最终返回的结果
          this.seller = Object.assign({}, this.seller, response.data)
          console.log(this.seller)
          console.log(this.seller.id)
        }
      })
    },
    components: {
      'v-header': header,
      'v-tab': tab
    }
  }
</script>

<style lang="less" type="text/less">
  @import "./common/less/index";

</style>
