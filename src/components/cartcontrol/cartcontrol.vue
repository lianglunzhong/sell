<template>
    <div class="cartcontrol">
      <transition name="move">
        <div class="cart-decrease" v-show="food.count > 0" @click.stop.prevent="decreaseCart">
          <span class="inner icon-remove_circle_outline"></span>
        </div>
      </transition>
      <transition name="fade">
        <div class="cart-count" v-show="food.count > 0">{{food.count}}</div>
      </transition>
      <div class="cart-add icon-add_circle" @click.stop.prevent="addCart"></div>
    </div>
</template>

<script type="text/ecmascript-6">
  import Vue from 'vue';
  import EventBus from '../../common/js/EventBus';
  
  export default {
    name: 'CartControl',
    data () {
      return {
        msg: '我是cartcontrol'
      };
    },
    props: {
      food: {
        type: Object
      }
    },
    methods: {
      addCart: function(event) {
        if (!this.food.count) {
          Vue.set(this.food, 'count', 1);
        } else {
          this.food.count++;
        }

        EventBus.$emit('cart.add', event.target);
      },
      decreaseCart: function() {
        if (this.food.count) {
          this.food.count--;
        }
      }
    }
  };
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="stylus" rel="stylesheet/stylus" scoped>
  @import "../../common/stylus/base"
  @import "../../common/stylus/mixin"
  @import "../../common/stylus/icon"
  
  .cartcontrol
    font-size: 0
    .cart-decrease, .cart-add
      display: inline-block
      padding: 6px
      .inner
        line-height: 24px
        font-size: 24px
        color: rgb(0, 160, 220)
      &.move-enter-active, &.move-leave-active
        transition: all .5s
        transform: translate3d(0, 0, 0)
        .inner
          transition: all .5s;
          transform: rotate(0deg)
      &.move-enter, &.move-leave-active 
        opacity: 0
        transform: translate3d(24px, 0, 0)
        .inner
          transform: rotate(180deg)
    .cart-count
      display: inline-block
      vertical-align: top
      width: 12px
      padding-top: 6px
      line-height: 24px
      text-align: center
      font-size: 10px
      color: rgb(147, 153, 159)
      &.fade-enter-active, &.fade-leave-active
        transition: opacity .5s
      &.fade-enter, &.fade-leave-to
        opacity: 0
    .cart-add
      display: inline-block
      padding: 6px
      line-height: 24px
      font-size: 24px
      color: rgb(0, 160, 220)
</style>
