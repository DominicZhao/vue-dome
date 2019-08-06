<template>
  <div class="goods">
    <div class="menu-wrapper" ref="menuWrapper">
      <ul>
        <li class="menu-list" v-for="(item, index) in goods" :key="item.id" :class="{'current':currentIndex==index}" @click="selectMenu(index,$event)">
          <span class="text">
            <supports-icon v-if="item.type>0" :size="24" :backgroundType="1" :type="item.type"></supports-icon>{{item.name}}
          </span>
        </li>
      </ul>
    </div>
    <div class="foods-wrapper" ref="foodWrapper">
      <ul>
        <li v-for="item in goods" :key="item.id" class="food-list food-list-hook">
          <h1 class="title">{{item.name}}</h1>
          <ul>
            <li v-for="food in item.foods" :key="food.id" class="food-item">
              <div class="icon">
                <img width="57" :src="food.icon" alt="">
              </div>
              <div class="content">
                <h2 class="food-name">{{food.name}}</h2>
                <p class="desc">{{food.description}}</p>
                <div class="extra">
                  <span class="sellCount">月售{{food.sellCount}}份</span>
                  <span class="rating">好评率{{food.rating}}%</span>
                </div>
                <div class="price">
                  <span class="now">
                    <span class="sign">￥</span>{{food.price}}
                  </span>
                  <span v-if="food.oldPrice" class="old">
                    <span class="sign">￥</span>{{food.oldPrice}}
                  </span>
                </div>
                <div class="cartcontrol-wrapper">
                  <cart-control :food="food" v-on:cart-add="_drop"></cart-control>
                </div>
              </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
    <shop-cart ref="shopcart" :delivery-price="seller.deliveryPrice" :min-price="seller.minPrice" :select-foods="selectFoods"></shop-cart>
  </div>
</template>

<script>
import SupportsIcon from '@/components/supports-icon/supports-icon';
import BScroll from 'better-scroll';
import ShopCart from '@/components/shop-cart/shop-cart';
import CartControl from '@/components/cart-control/cart-control';
const ERR_OK = 0;
export default {
  name: 'goods',
  props: {
    seller: Object
  },
  data () {
    return {
      goods: [],
      listHeight: [],
      scrollY: 0
    };
  },
  computed: {
    currentIndex () {
      for (let i = 0; i < this.listHeight.length; i++) {
        let heightBefore = this.listHeight[i];
        let heightAfter = this.listHeight[i + 1];
        if (!heightAfter || (this.scrollY >= heightBefore && this.scrollY < heightAfter)) {
          return i;
        }
      }
      return 0;
    },
    selectFoods () {
      let foods = [];
      this.goods.forEach((good) => {
        good.foods.forEach((food) => {
          if (food.count) {
            foods.push(food);
          }
        });
      });
      return foods;
    }
  },
  created () {
    this.$http.get('/api/goods').then(res => {
      res = res.body;
      if (res.errno === ERR_OK) {
        this.goods = res.data;
        this.$nextTick(() => {
          this._initScroll();
          this._calculateHegiht();
        });
      }
    });
  },
  components: {
    SupportsIcon,
    ShopCart,
    CartControl
  },
  methods: {
    selectMenu (index, $event) {
      if (!$event._constructed) {
        return;
      }
      let foodList = this.$refs.foodWrapper.getElementsByClassName('food-list-hook');
      let el = foodList[index];
      this.foodScroll.scrollToElement(el, 300);
    },
    _drop (target) {
      // 体验优化，异步执行小球下落动画
      this.$nextTick(() => {
        this.$refs['shopcart'].drop(target);
      });
    },
    _initScroll () {
      this.menuScroll = new BScroll(this.$refs.menuWrapper, {
        click: true
      });
      this.foodScroll = new BScroll(this.$refs.foodWrapper, {
        click: true,
        probeType: 3
      });
      this.foodScroll.on('scroll', (pos) => {
        this.scrollY = Math.abs(Math.round(pos.y));
      });
    },
    _calculateHegiht () {
      let foodList = this.$refs.foodWrapper.getElementsByClassName('food-list-hook');
      let height = 0;
      this.listHeight.push(height);
      for (let i = 0; i < foodList.length; i++) {
        let item = foodList[i];
        height += item.clientHeight;
        this.listHeight.push(height);
      }
    }
  },
  event: {
    'cart-add' (target) {
      this._drop(target);
    }
  }
};
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/public.styl"
.goods
  display: flex
  position: absolute
  top: 174px
  bottom: 46px
  left: 0
  width: 100%
  overflow: hidden
  .menu-wrapper
    flex: 0 0 80px
    width: 80px
    background-color: #f3f5f7
    .menu-list
      display: table
      height: 54px
      width: 56px
      line-height: 14px
      color: rgb(7,17,27)
      padding: 0 12px
      &.current
        position: relative
        z-index: 10
        margin-top: -1px
        background-color: #fff
        .text
          font-weight: 700
          border-none()
      .text
        display table-cell
        width 56px;
        vertical-align: middle
        font-size: 12px
        border-1px(rgba(7,17,27,0.1))
  .foods-wrapper
    flex: 1
    .title
      padding-left: 14px
      height: 26px
      line-height: 26px
      border-left: 2px solid #d9dde1
      font-size: 12px
      color: rgb(147,153,159)
      background-color: #f3f5f7
    .food-item
      display: flex
      margin:0 18px
      padding: 18px 0;
      border-1px(rgba(7,17,27,0.1))
      &:last-child
        border-none()
      .icon
        flex: 0 0 57px
        width: 57px
        margin-right: 10px
      .content
        flex: 1
        .food-name
          line-height: 14px
          font-size: 14px
          color: rgb(7,17,27)
          margin: 2px 0 8px 0
        .desc
          line-height: 12px
          font-size: 10px
          color: rgb(147,153,159)
          margin-bottom: 8px
        .extra
          font-size: 0
          margin-bottom: 8px
          .sellCount,.rating
            line-height: 10px
            font-size: 10px
            color: rgb(147,153,159)
          .rating
            margin-left: 12px
        .price
          font-weight: 700
          line-height: 24px
          .now
            line-height: 24px
            color: rgb(240,20,20)
            font-size: 14px
            font-weight: 700
            .sign
              font-size: 10px
              font-weight: normal
          .old
            text-decoration: line-through
            font-size: 10px
            color: rgb(147,153,159)
            font-weight: 700
            .sign
              font-weight: normal
        .cartcontrol-wrapper
          position: absolute
          right: 0
          bottom: 12px
</style>
