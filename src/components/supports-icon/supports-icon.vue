<template>
  <span :class="supportsClass">
    <span class="icon" :class="classMap[type]"></span>
  </span>
</template>

<script>
export default {
  name: 'supports-icon',
  props: {
    size: Number,
    type: Number,
    backgroundType: Number
  },
  computed: {
    supportsClass () {
      return ['support-' + this.size, 'support-' + this.supportIconType[this.backgroundType]];
    }
  },
  created () {
    this.supportIconType = ['transparent', 'background'];
    this.classMap = ['decrease', 'discount', 'guarantee', 'invoice', 'special'];
  }
};
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/public.styl"
.icon
  display: inline-block
  vertical-align: top
  background-repeat: no-repeat
.support-24
  .icon
    width: 12px
    height: 12px
    background-size: 12px 12px
  &.support-transparent
    .icon
      margin-right: 4px
      &.decrease
        bg-img('decrease_1')
      &.discount
        bg-img('discount_1')
      &.guarantee
        bg-img('guarantee_1')
      &.invoice
        bg-img('invoice_1')
      &.special
        bg-img('special_1')
  &.support-background
    .icon
      margin-right: 2px
      &.decrease
        bg-img('decrease_3')
      &.discount
        bg-img('discount_3')
      &.guarantee
        bg-img('guarantee_3')
      &.invoice
        bg-img('invoice_3')
      &.special
        bg-img('special_3')
.support-32
  .icon
    width: 16px
    height: 16px
    background-size: 16px 16px
  &.support-transparent
    .icon
      margin-right: 6px
      &.decrease
        bg-img('decrease_2')
      &.discount
        bg-img('discount_2')
      &.guarantee
        bg-img('guarantee_2')
      &.invoice
        bg-img('invoice_2')
      &.special
        bg-img('special_2')
  &.support-background
    .icon
      margin-right: 6px
      &.decrease
        bg-img('decrease_4')
      &.discount
        bg-img('discount_4')
      &.guarantee
        bg-img('guarantee_4')
      &.invoice
        bg-img('invoice_4')
      &.special
        bg-img('special_4')
</style>
