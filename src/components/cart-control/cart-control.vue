<template>
  <div class="cart-control">
    <transition name="move">
      <div class="cart-decrease" v-show="food.count>0" @click="reduceCart($event)">
        <i class="icon-remove_circle_outlin inner"></i>
      </div>
    </transition>
    <div class="cart-count" v-if="food.count>0">{{food.count}}</div>
    <div class="cart-add" @click="addCart($event)">
      <i class="icon-add_circle inner"></i>
    </div>
  </div>
</template>

<script>
import Vue from 'vue';
export default {
  name: 'cart-control',
  props: {
    food: Object
  },
  methods: {
    addCart ($event) {
      if (!$event._constructed) {
        return;
      }
      if (!this.food.count) {
        Vue.set(this.food, 'count', 1);
      } else {
        this.food.count++;
      }
      this.$emit('cart-add', $event.target);
    },
    reduceCart ($event) {
      if (!$event._constructed) {
        return;
      }
      if (this.food.count > 0) {
        this.food.count--;
      }
    }
  }
};
</script>

<style lang="stylus" rel="stylesheet/stylus">
.cart-control
  font-size: 0
  .cart-decrease
    display: inline-block
    padding: 6px
    .inner
      display: inline-block
      line-height: 24px
      font-size: 24px
      color: rgb(0,160,220)
      transition: all .5s linear;
    &.move-enter-active,&.move-leave-active
      transition: all .5s linear;
      opacity: 1
      transform: translate3d(0,0,0)
      .inner
        transform: rotate(0)
    &.move-enter,&.move-leave-to
      transition: all .5s linear;
      opacity: 0
      transform: translate3d(24px,0,0)
      .inner
        transform: rotate(180deg)
  .cart-count
    display: inline-block
    vertical-align: top
    width: 12px
    padding: 6px 0
    line-height: 24px
    text-align: center
    font-size: 10px
    color: rgb(147,153,159)
  .cart-add
    display: inline-block
    padding: 6px
    .inner
      line-height: 24px
      font-size: 24px
      color: rgb(0,160,220)
</style>
