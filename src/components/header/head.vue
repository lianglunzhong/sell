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
            <v-icon :size="12" :index="seller.supports[0].type"></v-icon>
            <!-- <span class="icon" v-bind:class="classMap[seller.supports[0].type]"></span> -->
            <span class="text">{{seller.supports[0].description}}</span>
          </div>
        </div>
        <div v-if="seller.supports" class="support-count" v-on:click="showDetail">
          <span class="count">{{seller.supports.length}}个</span>
          <i class="icon-keyboard_arrow_right"></i>
        </div>
      </div>
      <div class="bulletin-warpper" v-on:click="showDetail">
        <span class="bulletin-title"></span><span class="bulletin-text">{{seller.bulletin}}</span>
        <i class="icon-keyboard_arrow_right"></i>
      </div>
      <div class="background">
        <img v-bind:src="seller.avatar" width="100%" height="100%">
      </div>
      <transition name="fade">
        <div v-show="detailShow" class="detail">
          <div class="detail-wrapper clearfix">
            <div class="detail-main">
              <h1 class="name">{{seller.name}}</h1>
              <div class="star-wrapper">
                <v-star v-bind:size="48" v-bind:score="seller.score"></v-star>
              </div>
              <div v-if="seller.supports">
                <div class="title">
                  <div class="line"></div>
                  <div class="text">优惠信息</div>
                  <div class="line"></div>
                </div>
                <ul class="supports">
                  <li class="support-item" v-for="(item, index) in seller.supports" :key="index">
                    <!-- <span class="icon" :class="classMap[item.type]"></span> -->
                    <v-icon :size="16" :index="item.type"></v-icon>
                    <span class="text">{{item.description}}</span>
                  </li>
                </ul>
              </div>
              <div>
                <div class="title">
                  <div class="line"></div>
                  <div class="text">商家公告</div>
                  <div class="line"></div>
                </div>
                <div class="bulletin">
                  <p class="content">{{seller.bulletin}}</p>
                </div>
              </div>
            </div>
          </div>
          <div class="detail-close" v-on:click="hideDetail">
            <i class="icon-close"></i>
          </div>
        </div>
      </transition>
    </div>
</template>

<script type="text/ecmascript-6">
  import Star from '@/components/star/star';
  import Icon from '@/components/icon/icon';

  export default {
    name: 'Head',
    data () {
      return {
        msg: '我是header',
        detailShow: false
      };
    },
    props: {
      seller: {
        type: Object
      }
    },
    methods: {
      showDetail() {
        this.detailShow = true;
      },
      hideDetail() {
        this.detailShow = false;
      }
    },
    created() {
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
    },
    components: {
      'v-star': Star,
      'v-icon': Icon
    }
  };
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="stylus" rel="stylesheet/stylus" scoped>
  @import "../../common/stylus/base"
  @import "../../common/stylus/mixin"
  @import "../../common/stylus/icon"

  .header
    position: relative
    overflow: hidden
    background-color: rgba(7, 17, 27, 0.5)
    color:#fff
    .content-wrapper
      position: relative
      padding: 24px 12px 18px 24px
      font-size: 0px
      .avatar
        display: inline-block
        vertical-align: top
        img
          border-radius: 2px
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
        .description
          margin-bottom: 10px
          font-size: 12px
          line-height: 12px
        .supports
          .icon
            margin-right: 4px
          .text
            display: inline-block
            line-height: 10px
            font-size: 10px
      .support-count
        position: absolute
        vertical-align: top
        right: 12px
        bottom: 18px
        padding: 0 8px
        height: 24px
        lint-height: 24px
        border-radius: 14px
        background: rgba(0, 0, 0, 0.2)
        text-align: center
        .count
          font-size: 10px
        .icon-keyboard_arrow_right
          line-height: 24px
          margin-left: 2px
          font-size: 10px
    .bulletin-warpper
      position: relative
      height: 28px
      line-height: 28px
      padding 0 22px 0 12px
      overflow: hidden
      white-space: nowrap
      text-overflow: ellipsis
      background: rgba(7, 17, 27, 0.2)
      .bulletin-title
        display: inline-block
        vertical-align: middle
        width: 22px
        height: 12px
        bg-image('bulletin')
        background-size: 22px 12px
        background-repeat: no-repeat
      .bulletin-text
        font-size: 10px
        margin: 0 4px
      .icon-keyboard_arrow_right
        position: absolute
        font-size: 10px
        right: 10px
        top: 8px
    .background
      position: absolute
      top: 0
      left: 0
      width: 100%
      height: 100%
      z-index: -1
      filter: blur(10px)
    .detail
      position: fixed
      top: 0
      left: 0
      z-index: 100
      width: 100%
      height: 100%
      overflow: auto
      background: rgba(7, 17, 27, 0.8)
      backdrop-filter: blur(10px)
      .detail-wrapper
        width: 100%
        min-height: 100%
        .detail-main
          margin-top: 64px
          padding-bottom: 64px
          .name
            line-height: 16px
            text-align: center
            font-size: 16px
            font-weight: 700
          .star-wrapper
            margin-top: 18px
            padding: 2px 0
            text-align: center
          .title
            display: flex
            width:80%
            margin: 28px auto 24px auto
            .line
              flex: 1
              position: relative
              top: -6px;
              border-bottom 1px solid rgba(255, 255, 255, 0.2)
            .text
              padding: 0 12px;
              font-size: 14px
              font-weight: 700
          .supports
            width: 80%
            margin: 0 auto
            .support-item
              padding: 0 12px
              margin-bottom: 12px
              font-size: 0
              &:last-child
                margin-bottom: 0
              .icon
                margin-right: 6px;
              .text
                line-height: 12px
                font-size: 12px
                vertical-align: top
          .bulletin
            width: 80%
            margin: 0 auto
            .content
              padding: 0 12px
              line-height: 24px
              font-size: 12px
      .detail-close
        position: relative
        width: 32px
        height 32px
        margin: -64px auto 0 auto
        clear: both
        font-size: 32px
    .fade-enter-active, .fade-leave-active
      transition: all .5s
    .fade-enter, .fade-leave-to
      opacity: 0
</style>
