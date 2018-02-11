<template>
  <div>
  <div class="goods">
    <div class="menu-wrapper" ref="menu">
      <ul>
        <li v-for="(item, index) in goods" :key="index" class="menu-item" :class="{'current': currentIndex === index}" @click="selectMenu(index)">
          <span class="text">
            <v-icon v-if="item.type >= 0" :size="16" :index="item.type"></v-icon>
            {{item.name}}
          </span>
        </li>
      </ul>
    </div>
    <div class="foods-wrapper" ref="foods">
      <ul>
        <li v-for="(item, index) in goods" :key="index" class="food-list food-list-hook">
          <h1 class="title">{{item.name}}</h1>
          <ul>
            <li v-for="(food, index2) in item.foods" :key="index2" @click.stop.prevent="selectFood(food)" class="food-item">
              <div class="icon">
                <img width="57" height="57" :src="food.icon">
              </div>
              <div class="content">
                <h2 class="name">{{food.name}}</h2>
                <p class="desc">{{food.description}}</p>
                <div class="extra">
                  <span class="count">月售{{food.sellCount}}份</span><span class="rating">好评率{{food.rating}}%</span>
                </div>
                <div class="price">
                  <span class="now">￥{{food.price}}</span><span class="old" v-if="food.oldPrice">￥{{food.oldPrice}}</span>
                </div>
                <div class="cartcontrol-wrapper">
                  <v-cartcontrol :food="food"></v-cartcontrol>
                </div>
              </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
    <v-shopcart ref="shopcart" :select-foods="selectFoods" :delivery-price="seller.deliveryPrice" :min-price="seller.minPrice"></v-shopcart>
  </div>
  <v-food :food="selectedFood" ref="food"></v-food>
</div>
</template>

<script type="text/ecmascript-6">
  import EventBus from '../../common/js/EventBus';
  import Icon from '@/components/icon/icon';
  import ShopCart from '@/components/shopcart/shopcart';
  import CartControl from '@/components/cartcontrol/cartcontrol';
  import Food from '@/components/food/food';
  import BScroll from 'better-scroll';

  export default {
    name: 'Goods',
    data () {
      return {
        msg: '我是Goods',
        goods: [],
        listHeight: [],
        scrollY: 0,
        selectedFood: {}
      };
    },
    computed: {
      currentIndex: function () {
        for (let i = 0; i < this.listHeight.length; i++) {
          let height1 = this.listHeight[i];
          let height2 = this.listHeight[i + 1];
          if (!height2 || (this.scrollY >= height1 && this.scrollY < height2)) {
            return i;
          }
        }
        return 0;
      },
      selectFoods: function() {
        let foods = [];
        this.goods.forEach(function(good, i) {
          good.foods.forEach(function(food, j) {
            if (food.count) {
              foods.push(food);
            }
          });
        });

        return foods;
      }
    },
    props: {
      seller: {
        type: Object
      }
    },
    components: {
      'v-icon': Icon,
      'v-shopcart': ShopCart,
      'v-cartcontrol': CartControl,
      'v-food': Food
    },
    created() {
      this.$http.jsonp('http://127.0.0.1:8000/api/goods',
        {
          params: {},
          jsonp: 'callback'
        }).then(function(response) {
          if (response.body.status) {
            this.goods = response.body.data;
            this.$nextTick(function() {
              this._initScroll();
              this._calculateHeight();
            });
          }
        }, function(response) {
          console.log(response);
      });

      let self = this;
      EventBus.$on('cart.add', function(el) {
        self.$nextTick(function() {
          self.$refs.shopcart.drop(el);
        });
      });
    },
    methods: {
      _initScroll() {
        this.menuScroll = new BScroll(this.$refs.menu, {
          click: true
        });
        this.foodsScroll = new BScroll(this.$refs.foods, {
          click: true,
          probeType: 3
        });

        let self = this;
        this.foodsScroll.on('scroll', function(pos) {
          self.scrollY = Math.abs(Math.round(pos.y));
        });
      },
      _calculateHeight() {
        let foodList = this.$refs.foods.getElementsByClassName('food-list-hook');
        let height = 0;
        this.listHeight.push(height);
        for (let i = 0; i < foodList.length; i++) {
          let item = foodList[i];
          height += item.clientHeight;
          this.listHeight.push(height);
        }
      },
      selectMenu(index) {
        let foodList = this.$refs.foods.getElementsByClassName('food-list-hook');
        let el = foodList[index];

        this.foodsScroll.scrollToElement(el, 300);
      },
      selectFood(food) {
        this.selectedFood = food;
        this.$refs.food.show();
      }
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
        &.current
          position: relative
          z-index: 10
          background: #fff
          font-weight: 700
          .text
            border-none()
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
      .title
        padding-left: 14px
        height: 26px
        line-height: 26px
        border-left: 2px solid #d9dde1
        font-size: 12px
        color: rgb(147, 153, 159)
        background: #f3f5f7
      .food-item
        display: flex
        margin: 18px
        padding-bottom: 18px
        border-1px(rgba(7, 17, 27, .1))
        &:last-child
          border-none()
          margin-bottom: 0
        .icon
          flex: 0 0 57px
          width: 57px
          margin-right: 10px
        .content
          flex: 1
          .name
            margin: 2px 0 8px 0
            height: 14px
            line-height: 14px
            font-size: 14px
            color: rgb(7, 17, 27)
          .desc, .extra
            line-height: 10px
            font-size: 10px
            color: rgb(147, 153, 159)
          .desc
            margin-bottom: 8px
            line-height: 12px
          .extra
            .count
              margin-right: 12px
          .price
            font-weight: 700
            line-height: 24px
            .now
              margin-right: 18px
              font-size: 14px
              color: rgb(240, 20, 20)
            .old
              text-decoration: line-through
              font-size: 10px
              color: rgb(147, 153, 159)        
          .cartcontrol-wrapper
            position: absolute
            right: 0
            bottom: 12px
</style>
