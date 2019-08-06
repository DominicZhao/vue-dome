<template>
  <div id="headers">
    <div class="index-header">
      <div class="thumbnail">
        <img width="64" height="64" :src="seller.avatar" alt="">
      </div>
      <div class="commodity-synopsis">
        <h2 class="title">
          <span class="icon"></span>
          <strong class="name">{{seller.name}}</strong>
        </h2>
        <h3 class="distribution-info">
          {{seller.description}}/{{seller.deliveryTime}}分钟送达
        </h3>
        <h4 v-if="seller.supports" class="support">
          <supports-icon :size="24" :backgroundType="0" :type="seller.supports[0].type"></supports-icon>
          <span class="support-info">{{seller.supports[0].description}}</span>
        </h4>
      </div>
      <div v-if="seller.supports" class="support-count" @click="showDetail">
        <span class="count">{{seller.supports.length}}个</span>
        <i class="icon-keyboard_arrow_right"></i>
      </div>
    </div>
    <div class="notice-info" @click="showDetail">
      <span class="icon"></span><span class="text">{{seller.bulletin}}</span>
      <i class="icon-keyboard_arrow_right"></i>
    </div>
    <div class="background">
      <img :src="seller.avatar" alt="">
    </div>
    <transition name="fade">
      <div v-show="detailShow" class="detail">
        <div class="detail-wrapper clearfix">
          <div class="detail-main">
            <h1 class="seller-name">{{seller.name}}</h1>
            <div class="star-wrapper">
              <stars :size="48" :score="seller.score"></stars>
            </div>
            <line-title :titleType="0"></line-title>
            <ul class="discount-info" v-if="seller.supports">
              <li class="info-cell" v-for="item in seller.supports" :key="item.id">
                <supports-icon :size="32" :backgroundType="0" :type="item.type"></supports-icon>
                <strong class="text">{{item.description}}</strong>
              </li>
            </ul>
            <line-title :titleType="1"></line-title>
            <div class="bulletin">
              <p class="text">{{seller.bulletin}}</p>
            </div>
          </div>
        </div>
        <div class="detail-close" @click="detailHide">
          <i class="icon-close"></i>
        </div>
      </div>
    </transition>
  </div>
</template>

<script>
import Stars from '@/components/stars/stars';
import LineTitle from '@/components/line-title/line-title';
import SupportsIcon from '@/components/supports-icon/supports-icon';

export default {
  name: 'headers',
  props: {
    seller: Object
  },
  data () {
    return {
      detailShow: false
    };
  },
  methods: {
    showDetail () {
      this.detailShow = true;
    },
    detailHide () {
      this.detailShow = false;
    }
  },
  components: {
    Stars,
    LineTitle,
    SupportsIcon
  }
};
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/public.styl"
#headers
  position: relative
  background-color: rgba(7,17,27,0.5)
  overflow: hidden
  color: rgb(255,255,255)
  .index-header
    padding: 24px 12px 18px 24px
    font-size: 0px
    position: relative
    .thumbnail
      display: inline-block
      vertical-align: top
      img
        border-radius: 4px
    .commodity-synopsis
      display: inline-block
      margin-left: 16px
      .title
        line-height: 18px
        font-size: 16px
        margin: 2px 0 8px 0
        .icon
          display: inline-block
          vertical-align: top
          margin-right: 6px
          width: 30px
          height: 18px
          bg-img('brand')
          background-repeat: no-repeat
          background-size: 30px 18px
        .name
          font-weight: bold
      .distribution-info
        margin-bottom: 10px
        line-height: 12px
        font-size: 12px
      .support
        margin-bottom: 2px
        line-height: 12px
        font-size: 10px
    .support-count
      position absolute
      right: 12px
      bottom: 14px
      padding: 0 8px
      height: 24px
      line-height: 24px
      font-size: 10px
      text-align: center
      border-radius: 14px
      background-color: rgba(0,0,0,0.2)
      .count
        vertical-align: top
        font-size: 10px
      .icon-keyboard_arrow_right
        margin-left: 2px
        line-height: 24px
        font-size: 10px
  .notice-info
    padding: 0 22px 0 12px
    height: 28px
    line-height: 28px
    white-space: nowrap
    overflow: hidden
    text-overflow: ellipsis
    position: relative
    background-color: rgba(7,17,27,0.2)
    .icon
      display: inline-block
      vertical-align: top
      margin-top: 8px
      width: 22px
      height: 12px
      bg-img('bulletin')
      background-repeat: no-repeat
      background-size: 22px 12px
    .text
      margin: 0 4px
      font-size: 10px
    .icon-keyboard_arrow_right
      position: absolute
      right: 12px
      top: 9px;
      font-size: 10px
  .background
    position: absolute
    top: 0
    left: 0
    width: 100%
    height: 100%
    z-index: -1
    img
      width: 100%
      filter:blur(10px)
  .fade-enter-active, .fade-leave-active
    transition: all .5s ease;
    opacity: 1
  .fade-enter,.fade-leave-to
    transition: all .5s ease;
    opacity: 0
  .detail
    position: fixed
    top: 0
    left: 0
    z-index: 100
    width: 100%
    height: 100%
    overflow: auto
    background-color: rgba(7,17,27,0.8)
    backdrop-filter: blur(10)
    .detail-wrapper
      min-height: 100%
      width: 100%
      .detail-main
        margin-top: 64px
        padding-bottom: 64px
        .seller-name
          line-height: 16px
          text-align: center
          font-size: 16px
          font-weight: 700
        .star-wrapper
          margin-top: 16px
          padding: 2px 0
          text-align: center
        .discount-info
          width: 84%
          margin: 0 auto
          .info-cell + .info-cell
            margin-top: 12px
          .info-cell
            padding: 0 12px
            font-size: 0
            .text
              line-height: 16px
              font-size: 12px
        .bulletin
          width: 84%
          margin: 0 auto
          .text
            line-height: 24px
            padding: 0 12px
            font-size 12px
    .detail-close
      position: relative
      width: 32px
      height: 32px
      margin: -64px auto 0;
      clear: both
      font-size: 32px
</style>
