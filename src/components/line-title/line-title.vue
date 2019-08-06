<template>
  <div class="line-title">
    <div class="line"></div>
    <div class="title-text">{{supportTitle[titleType]}}</div>
    <div class="line"></div>
  </div>
</template>

<script>
export default {
  name: 'line-title',
  props: {
    titleType: Number
  },
  created () {
    this.supportTitle = ['优惠信息', '商家公告'];
  }
};
</script>

<style lang="stylus" rel="stylesheet/stylus">
.line-title
  display: flex
  width: 84%
  margin: 28px auto 24px
  .line
    flex: 1
    position: relative
    top: -6px
    border-bottom: 1px solid rgba(255,255,255,0.2)
  .title-text
    padding: 0 12px
    line-height: 14px
    font-size: 14px
    font-weight: 700
</style>
