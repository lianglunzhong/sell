<template>
  <div class="goods">
    <div class="menu-wrapper">
      <li v-for="(item, index) in goods" :key="index" class="menu-item">
        <span class="text">
          <v-icon v-if="item.type >= 0" :size="16" :index="item.type"></v-icon>
          {{item.name}}
        </span>
      </li>
    </div>
    <div class="foods-wrapper"></div>
  </div>
</template>

<script type="text/ecmascript-6">
  import Icon from '@/components/icon/icon';
  export default {
    name: 'Goods',
    data () {
      return {
        msg: '我是Goods',
        goods: []
      };
    },
    props: {
      seller: {
        type: Object
      }
    },
    components: {
      'v-icon': Icon
    },
    created() {
      this.$http.jsonp('http://127.0.0.1:8000/api/goods',
        {
          params: {},
          jsonp: 'callback'
        }).then(function(response) {
          if (response.body.status) {
            this.goods = response.body.data;
            console.log(this.goods);
          }
        }, function(response) {
          console.log(response);
      });
    }
  };
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="stylus" rel="stylesheet/stylus" scoped>
  @import "../../common/stylus/base"
  @import "../../common/stylus/mixin"
  @import "../../common/stylus/icon"
  
  .goods
    display: flex
    position: absolute
    top: 174px
    bottom: 46px
    width: 100%
    overflow: hidden
    .menu-wrapper
      flex: 0 0 80px
      width: 80px
      background: #f3f5f7
      .menu-item
        display: table
        width: 56px
        height: 54px
        padding: 0 12px
        line-height: 14px
        .text
          display: table-cell
          width 56px
          vertical-align: middle
          border-1px(rgba(7, 17, 27, .1))
          font-size: 12px
          .icon
            margin-right: 2px
    .foods-wrapper
      flex: 1
</style>
