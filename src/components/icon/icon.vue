<template>
    <span class="icon" v-bind:class="[iconType, classMap[index]]"></span>
</template>

<script type="text/ecmascript-6">
  export default {
    name: 'Icon',
    data () {
      return {
        classMap: ['decrease', 'discount', 'special', 'invoice', 'guarantee']
      };
    },
    props: {
      size: {
        type: Number
      },
      index: {
        type: Number
      }
    },
    computed: {
      iconType() {
        return 'icon-' + this.size;
      }
    }
  };
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="stylus" rel="stylesheet/stylus" scoped>
  @import "../../common/stylus/base"
  @import "../../common/stylus/mixin"
  @import "../../common/stylus/icon"
  
  .icon
    display: inline-block
    vertical-align: middle
    background-repeat: no-repeat
    &.icon-12
      width: 12px
      height: 12px
      background-size: 12px 12px
      &.decrease
        bg-image('decrease_1')
      &.discount
        bg-image('discount_1')
      &.guarantee
        bg-image('guarantee_1')
      &.invoice
        bg-image('invoice_1')
      &.special
        bg-image('special_1')
    &.icon-16
      width: 16px
      height: 16px
      background-size: 16px 16px
      &.decrease
        bg-image('decrease_2')
      &.discount
        bg-image('discount_2')
      &.guarantee
        bg-image('guarantee_2')
      &.invoice
        bg-image('invoice_2')
      &.special
        bg-image('special_2')
    &.icon-13
      width: 12px
      height: 12px
      background-size: 12px 12px
      &.decrease
        bg-image('decrease_3')
      &.discount
        bg-image('discount_3')
      &.guarantee
        bg-image('guarantee_3')
      &.invoice
        bg-image('invoice_3')
      &.special
        bg-image('special_3')
</style>
