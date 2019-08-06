<template>
  <div id="app">
    <v-header :seller="seller"></v-header>
    <ul class="tab border-1px">
      <li class="tab-item">
        <router-link to="/goods" active-class="active">商品</router-link>
      </li>
      <li class="tab-item">
        <router-link to="/ratings" active-class="active">评价</router-link>
      </li>
      <li class="tab-item">
        <router-link to="/sellers" active-class="active">商家</router-link>
      </li>
    </ul>
    <router-view></router-view>
  </div>
</template>

<script>
import header from '@/components/header/header';

const ERR_OK = 0;

export default {
  name: 'app',
  data () {
    return {
      seller: { }
    };
  },
  created: function () {
    this.$http.get('api/seller').then(res => {
      res = res.body;
      if (res.errno === ERR_OK) {
        this.seller = res.data;
      }
    });
  },
  components: {
    'v-header': header
  }
};
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "common/stylus/public.styl"
  #app
    .tab
      display: flex
      width:100%
      height: 40px
      line-height: 40px
      background-color: rgb(255,255,255)
      //border-bottom : 1px solid rgba(7,17,27,0.1)
      border-1px(rgba(7,17,27,0.1))
      .tab-item
        flex: 1
        text-align: center
        &>a
          display: block
          font-size: 14px
          color: rgb(77,85,93)
          &.active
            color:rgb(240,20,20)
</style>
