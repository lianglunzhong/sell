<template>
  <div id="app">
    <v-head v-bind:seller="seller"></v-head>
    <div class="tab">
      <div class="tab-item">
        <router-link to="/goods">商品</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/ratings">评价</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/seller">商家</router-link>
      </div>
    </div>
    <keep-alive>
      <router-view :seller="seller"></router-view>
    </keep-alive>
  </div>
</template>

<script type="text/ecmascript-6">
  import Head from '@/components/header/head';

  export default {
    name: 'App',
    components: {
      'v-head': Head
    },
    data() {
      return {
        seller: {
          id: '12312'
        }
      };
    },
    created() {
      this.$http.jsonp('http://127.0.0.1:8000/api/seller',
        {
          params: {},
          jsonp: 'callback'
        }).then(function(response) {
          if (response.body.status) {
            // this.seller = response.body.data;
            // 相当于 extend方法 扩展  es6语法   vue推荐的给对象扩展属性方法
            this.seller = Object.assign({}, this.seller, response.body.data);
          }
        }, function(response) {
          console.log(response);
      });
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus" scoped>
  @import "common/stylus/mixin"
  #app
    .tab
      display: flex
      width: 100%
      height: 40px
      line-height: 40px
      // border-bottom: 1px solid rgba(7, 17, 27, 0.1)
      border-1px(rgba(7, 17, 27, 0.1))
      .tab-item
        flex: 1
        text-align: center
        & > a
          display: block
          font-size: 28px
          color: rgb(77, 85, 93)
          &.active
            color: rgb(240, 20, 20)
</style>
