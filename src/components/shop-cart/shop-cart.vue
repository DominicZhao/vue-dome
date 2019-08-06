<template>
  <div class="shop-cart">
    <div class="content" @click="toggleList">
      <div class="content-left">
        <div class="logo-wrapper">
          <div class="logo" :class="{'heighlight':totalCount>0}">
            <i class="icon-shopping_cart"></i>
          </div>
          <div v-if="totalCount>0" class="num">{{totalCount}}</div>
        </div>
        <div class="price" :class="{'heighlight':totalPrice>0}">￥{{totalPrice}}</div>
        <div class="desc">另需配送费￥{{deliveryPrice}}元</div>
      </div>
      <div class="content-right">
        <!--<div class="pay fail-pay" v-if="!totalPrice">
          ￥{{minPrice}}起送
        </div>
        <div class="pay fail-pay" v-if="totalPrice<minPrice && totalPrice>0">
          还差￥{{minPrice-totalPrice}}起送
        </div>
        <div class="pay reach-pay" v-if="totalPrice>=minPrice">
          去结算
        </div>-->
        <div class="pay" :class="payClass">
          {{payText}}
        </div>
      </div>
    </div>
    <div class="ball-container">
      <transition-group name="drop" tag="div" v-on:before-enter="beforeEnter" v-on:enter="enter" v-on:after-enter="afterEnter">
        <div v-for="(ball,index) in balls" :key="index" v-show="ball.show" class="ball">
          <div class="inner inner-hook"></div>
        </div>
      </transition-group>
    </div>
    <transition name="fold">
      <div class="shopcart-list" v-show="listShow">
        <div class="list-header">
          <h1 class="title">购物车</h1>
          <span class="empty">清空</span>
        </div>
        <div class="list-content" ref="listContent">
          <ul>
            <li class="food" v-for="(food, index) in selectFoods" :key="index">
              <span class="name">{{food.name}}</span>
              <div class="price">
                <span>￥{{food.price*food.count}}</span>
              </div>
              <div class="cartcontrol-wrapper">
                <cart-control :food="food"></cart-control>
              </div>
            </li>
          </ul>
        </div>
      </div>
    </transition>
  </div>
</template>

<script>
import BScroll from 'better-scroll';
import CartControl from '@/components/cart-control/cart-control';
export default {
  name: 'shop-cart',
  props: {
    deliveryPrice: {
      type: Number,
      default: 0
    },
    minPrice: {
      type: Number,
      default: 0
    },
    selectFoods: {
      type: Array,
      default: function () {
        return [];
      }
    }
  },
  data () {
    return {
      balls: [
        {
          show: false
        },
        {
          show: false
        },
        {
          show: false
        },
        {
          show: false
        },
        {
          show: false
        }
      ],
      dropBalls: [],
      fold: true
    };
  },
  computed: {
    totalPrice () {
      let total = 0;
      this.selectFoods.forEach((food) => {
        total += food.count * food.price;
      });
      return total;
    },
    totalCount () {
      let count = 0;
      this.selectFoods.forEach((food) => {
        count += food.count;
      });
      return count;
    },
    payText () {
      if (this.totalPrice === 0) {
        return `￥${this.minPrice}起送`;
      } else if (this.totalPrice < this.minPrice && this.totalPrice > 0) {
        let diff = this.minPrice - this.totalPrice;
        return `还差￥${diff}起送`;
      } else {
        return '去结算';
      }
    },
    payClass () {
      if (this.totalPrice > this.minPrice) {
        return 'reach-pay';
      } else {
        return 'fail-pay';
      }
    },
    listShow () {
      let fold = this.fold;
      if (!this.totalCount) {
        fold = true;
        return false;
      }
      let show = !fold;
      if (show) {
        this.$nextTick(() => {
          if (!this.scroll) {
            /* eslint-disable-next-line */
            this.scroll = new BScroll(this.$refs.listContent, {
              click: true
            });
          } else {
            this.scroll.refresh();
          }
        });
      }
      return show;
    }
  },
  methods: {
    drop (el) {
      for (let i = 0; i < this.balls.length; i++) {
        let ball = this.balls[i];
        if (!ball.show) {
          ball.show = true;
          ball.el = el;
          this.dropBalls.push(ball);
          return;
        }
      }
    },
    // --------
    // 进入中
    // --------
    beforeEnter: function (el) {
      let count = this.balls.length;
      while (count--) {
        let ball = this.balls[count];
        if (ball.show) {
          let rect = ball.el.getBoundingClientRect();
          let X = rect.left - 32;
          let Y = -(window.innerHeight - rect.top - 22);
          el.style.display = '';
          el.style.webkitTransform = `translate3d(0,${Y}px,0)`;
          el.style.transform = `translate3d(0,${Y}px,0)`;
          let inner = el.getElementsByClassName('inner-hook')[0];
          inner.style.webkitTransform = `translate3d(${X}px,0,0)`;
          inner.style.transform = `translate3d(${X}px,0,0)`;
        }
      }
    },
    // 此回调函数是可选项的设置
    // 与 CSS 结合时使用
    enter: function (el, done) {
      /* eslint-disable no-unused-vars */
      let rf = el.offsetHeight;
      this.$nextTick(() => {
        el.style.webkitTransform = 'translate3d(0,0,0)';
        el.style.transform = 'translate3d(0,0,0)';
        let inner = el.getElementsByClassName('inner-hook')[0];
        inner.style.webkitTransform = 'translate3d(0,0,0)';
        inner.style.transform = 'translate3d(0,0,0)';
        el.addEventListener('transitionend', done);
      });
    },
    afterEnter: function (el) {
      let ball = this.dropBalls.shift();
      if (ball) {
        ball.show = false;
        el.style.display = 'none';
        ball.el = null;
      }
    },
    toggleList () {
      if (!this.totalCount) {
        return;
      }
      this.fold = !this.fold;
    }
  },
  components: {
    CartControl
  }
};
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/public.styl"
.shop-cart
  position: fixed
  bottom: 0
  left: 0
  z-index: 99
  width: 100%
  height: 48px
  background-color: rgba(7,17,27,0.95)
  .content
    display: flex
    height: 48px
    color: rgba(255,255,255,0.4)
    background-color: rgba(7,17,27,0.95)
    .content-left
      flex: 1
      font-size: 0
      .logo-wrapper
        display: inline-block
        position: relative
        top: -10px
        margin: 0 12px
        padding: 6px
        width: 56px
        height: 56px
        box-sizing: border-box
        border-radius: 50%
        vertical-align: top
        background-color: rgba(7,17,27,0.95)
        .logo
          width: 100%
          height: 100%
          border-radius: 50%
          text-align: center
          background-color: rgba(255,255,255,0.1)
          &.heighlight
            background-color: rgb(0,160,220)
            .icon-shopping_cart
              color: #fff
          .icon-shopping_cart
            line-height: 44px
            font-size: 24px
        .num
          position: absolute
          top: 0
          right: 0
          width: 24px
          height: 16px
          padding: 0 6px
          line-height: 16px
          font-size: 9px
          color: #fff
          border-radius: 16px
          font-weight: 700
          text-align: center
          background-color: rgb(240,20,20)
          box-sizing: border-box
          box-shadow: 0px 4px 8px 0px rgba(0,0,0,0.4)
      .price
        display: inline-block
        font-size: 16px
        font-weight: 700
        margin: 12px 0
        padding-right: 12px
        line-height: 24px
        box-sizing: border-box
        border-right 1px solid rgba(255,255,255,0.1)
        &.heighlight
          color: #fff
      .desc
        display: inline-block
        font-size: 10px
        margin: 12px
        line-height: 24px
    .content-right
      flex: 0 0 105px
      width: 105px
      .pay
        padding: 0 16px
        text-align: center
        line-height: 48px
        font-size: 12px
        &.fail-pay
          background-color: #2b333b
        &.reach-pay
          font-weight: 700
          color: #fff
          background-color: #00b43c
  .ball-container
    .ball
      position: fixed
      left: 32px
      bottom: 22px
      z-index: 200
      .inner
        width 16px
        height 16px
        border-radius 50%
        background-color rgb(0,160,220)
        transition: all 1s linear;
      &.drop-enter-active
        transition: all 1s cubic-bezier(.66,-0.74,.85,.56);
  .shopcart-list
    position: absolute
    left: 0
    top: 0
    z-index: -1
    width: 100%
    transform: translate3d(0,-100%,0)
    &.fold-enter-active,&.fold-leave-active
      transition: all 0.5s linear
      transform: translate3d(0,-100%,0)
    &.fold-enter,&.fold-leave-to
      transition: all 0.5s linear
      transform: translate3d(0,0,0)
    .list-header
      height: 40px
      line-height: 40px
      padding: 0 18px
      background-color: #f3f5f7
      border-bottom: 1px solid rgba(7,17,27,0.1)
      .title
        float: left
        font-size: 14px
        color: rgb(7,17,27)
      .empty
        float: right
        font-size: 12px
        color: rgb(0,160,220)
    .list-content
      padding: 0 18px
      max-height: 217px
      overflow: hidden
      background-color: #fff
      .food
        position: relative
        padding: 12px 0
        box-sizing: border-box
        border-1px(rgba(7,17,27,0.1))
        .name
          line-height: 24px
          font-size: 14px
          color: rgb(7,17,27)
        .price
          position: absolute
          right: 90px
          bottom: 12px
          line-height: 24px
          font-size: 14px
          font-weight: 700
          color: rgb(240,20,20)
        .cartcontrol-wrapper
          position: absolute
          right: 0
          bottom: 6px
</style>
