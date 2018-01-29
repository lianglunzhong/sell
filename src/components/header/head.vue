<template>
    <div class="header">
      <div class="content-wrapper">
        <div class="avatar">
          <img width="64" height="64" v-bind:src="seller.avatar">
        </div>
        <div class="content">
          <div class="title">
            <span class="brand"></span>
            <span class="name">{{seller.name}}</span>
          </div>
          <div class="description">
            {{seller.description}}/{{seller.deliveryTime}}分钟送达
          </div>
          <div v-if="seller.supports" class="supports">
            <span class="icon"></span>
            <span class="text">{{seller.supports[0].description}}</span>
          </div>
        </div>
      </div>
      <div class="bulletin-warpper"></div>
    </div>
</template>

<script type="text/ecmascript-6">
  export default {
    name: 'Head',
    data () {
      return {
        msg: '我是header'
      };
    },
    props: {
      seller: {
        type: Object
      }
    }
  };
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="stylus" rel="stylesheet/stylus" scoped>
  @import "../../common/stylus/mixin"
  .header
    color:#fff
    background: #ccc
    .content-wrapper
      padding: 24px 12px 18px 24px
      font-size: 0px
      .avatar
        display: inline-block
      .content
        display: inline-block
        margin-left: 16px
        font-size: 14px
      .title
        margin: 2px 0 8px 0
        .brand
          display: inline-block
          vertical-align: top
          width: 30px
          height: 18px
          bg-image('brand')
          background-size: 30px 18px
          background-repeat: no-repeat
        .name
          margin-left: 6px
          font-size: 16px
          line-height: 18px
          font-weight: bold
</style>
