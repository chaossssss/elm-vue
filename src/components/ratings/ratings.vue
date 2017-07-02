<template>
  <div class="ratings" ref="ratings">
    <div class="ratings-content">
      <div class="overview">
        <div class="overview-left">
          <h1 class="score">{{seller.score}}</h1>
          <div class="title">综合评分</div>
          <div class="rank">高于周边商家{{seller.rankRate}}%</div>
        </div>
        <div class="overview-right">
          <div class="score-wrapper">
            <div class="title">服务态度</div>
            <star class="star" :size="36" :score="seller.serviceScore"></star>
            <span class="score">{{seller.serviceScore}}</span>
          </div>
          <div class="score-wrapper">
            <span class="title">商品评分</span>
            <star class="star" :size="36" :score="seller.foodScore"></star>
            <span class="score">{{seller.foodScore}}</span>
          </div>
          <div class="delivery-wrapper">
            <span class="title">送到时间</span>
            <span class="delivery">{{seller.deliveryTime}}分钟</span>
          </div>
        </div>
      </div>
      <split></split>
      <ratingselect @select="selectRating" @toggle="toggleContent" :selectType="selectType" :onlyContent="onlyContent" :desc="desc" :ratings="ratings"></ratingselect>
      <div class="rating-wrapper">
        <ul>
          <li v-for="ratingItem in ratings" v-show="needShow(ratingItem.rateType, ratingItem.text)" class="rating-item border-1px">
            <div class="avatar">
              <img width="28" height="28" :src="ratingItem.avatar">
            </div>
            <div class="content">
              <h1 class="name">{{ratingItem.username}}</h1>
              <div class="star-wrapper">
                <star :size="24" :score="ratingItem.score"></star>
                <span class="delivery" v-show="ratingItem.deliveryTime"></span>
              </div>
              <p class="text">{{ratingItem.text}}</p>
              <div class="recommend">
                <span :class="{'icon-thumb_up':ratingItem.rateType === 0,'icon-thumb_down':ratingItem.rateType === 1}"></span>
                <span class="item" v-for="item in ratingItem.recommend">{{item}}</span>
              </div>
              <div class="time">{{ratingItem.rateTime | formatDate}}</div>
            </div>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
import star from '../star/star'
import split from '../split/split'
import ratingselect from '../ratingselect/ratingselect'
import BScroll from 'better-scroll'
import {formatDate} from '../../common/js/date'
const POSITIVE = 0
const NEGATIVE = 1
const ALL = 2
const ERR_OK = 0
export default {
  data () {
    return {
      onlyContent: false,
      desc: {
        all: '全部',
        positive: '满意',
        negative: '不满意'
      },
      ratings: [],
      selectType: ALL
    }
  },
  created () {
    this.$http.get('/api/ratings').then((response) => {
      response = response.body
      if (response.errno === ERR_OK) {
        this.ratings = response.data
        this.$nextTick(() => {
          this.scroll = new BScroll(this.$refs.ratings, {
            click: true
          })
        })
      }
    })
  },
  props: {
    seller: {
      type: Object
    }
  },
  methods: {
    needShow (type, text) {
      if (this.onlyContent && !text) {
        return false
      }
      if (this.selectType === ALL) {
        return true
      } else {
        return type === this.selectType
      }
    },
    selectRating (type) {
      this.selectType = type
      this.$nextTick(() => {
        this.scroll.refresh()
      })
    },
    toggleContent () {
      this.onlyContent = !this.onlyContent
      this.$nextTick(() => {
        this.scroll.refresh()
      })
    }
  },
  components: {
    star,
    split,
    ratingselect
  },
  filters: {
    formatDate (time) {
      let date = new Date(time)
      return formatDate(date, 'yyyy-MM-dd hh:mm')
    }
  }
}
</script>

<style lang="stylus" rel="stylesheet/stylus" scoped>
@import './css/ratings.styl'
</style>
