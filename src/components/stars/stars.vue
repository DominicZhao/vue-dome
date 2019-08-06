<template>
  <ul class="star" :class="starType">
    <li v-for="item in starsClass" :key="item.id" class="star-item" :class="item.class" track-by="$index"></li>
  </ul>
</template>

<script>
const LENGTH = 5;
const CLS_ON = 'star-on';
const CLS_HALF = 'star-half';
const CLS_OFF = 'star-off';
export default {
  name: 'stars',
  props: {
    size: Number,
    score: Number
  },
  computed: {
    starType () {
      return 'star-' + this.size;
    },
    starsClass () {
      let result = [];
      let score = Math.floor(this.score * 2) / 2;
      let hasDecimal = score % 1 !== 0;
      let integer = Math.floor(score);
      for (let i = 0; i < integer; i++) {
        result.push({id: i + 1, class: CLS_ON});
      }
      if (hasDecimal) {
        result.push({id: result.length + 1, class: CLS_HALF});
      }
      while (result.length < LENGTH) {
        result.push({id: result.length + 1, class: CLS_OFF});
      }
      return result;
    }
  }
};
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/public.styl"
.star
  font-size: 0
  .star-item
    display: inline-block
    background-repeat: no-repeat
  &.star-24
    .star-item
      width: 10px
      height: 10px
      background-size: 10px 10px
    .star-item + .star-item
      margin-left: 3px
    .star-on
      bg-img("star24_on")
    .star-half
      bg-img("star24_half")
    .star-off
      bg-img("star24_off")
  &.star-36
    .star-item
      width: 15px
      height: 15px
      background-size: 15px 15px
    .star-item + .star-item
      margin-left: 16px
    .star-on
      bg-img("star36_on")
    .star-half
      bg-img("star36_half")
    .star-off
      bg-img("star36_off")
  &.star-48
    .star-item
      width: 20px
      height: 20px
      background-size: 20px 20px
    .star-item + .star-item
      margin-left: 22px
    .star-on
      bg-img("star48_on")
    .star-half
      bg-img("star48_half")
    .star-off
      bg-img("star48_off")
</style>
