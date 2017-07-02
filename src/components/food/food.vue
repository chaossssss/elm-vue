<template>
  <transition name="slide">
    <div v-show="showFlag" class="food" ref="food">
      <div class="food-content">
        <div class="image-header">
          <img :src="food.image">
          <div class="back" @click="hide">
            <i class="icon-arrow_lift"></i>
          </div>
        </div>
        <div class="content">
          <h1 class="title">{{food.name}}</h1>
          <div class="detail">
            <span class="sell-count">月售{{food.sellCount}}份</span>
            <span class="rating">好评率{{food.rating}}%</span>
          </div>
          <div class="price">
            <span class="now">￥{{food.price}}</span>
            <span class="old" v-show="food.oldPrice">￥{{food.oldPrice}}</span>
          </div>
          <div class="cartcontrol-wrapper">
            <cartcontrol :food="food"></cartcontrol>
          </div>
          <transition name="fade">
            <div @click.stop.prevent="addFirst" class="buy" v-show="!food.count || food.count === 0">加入购物车</div>
          </transition>
        </div>
        <split v-show="food.info"></split>
        <div class="info" v-show="food.info">
          <h1 class="title">商品信息</h1>
          <p class="text">{{food.info}}</p>
        </div>
        <split></split>
        <div class="rating">
          <h1 class="title">商品评价</h1>
          <ratingselect @toggle="toggleContent" @select="selectRating" :selectType="selectType" :onlyContent="onlyContent" :desc="desc" :ratings="food.ratings"></ratingselect>
          <div class="rating-wrapper">
            <ul>
              <li v-for="ratingItem in food.ratings" v-show="needShow(ratingItem.rateType,ratingItem.text)" class="rating-item border-1px">
                <div class="user">
                  <span class="name">{{ratingItem.username}}</span>
                  <img class="avatar" width="12" height="12" :src="ratingItem.avatar">
                </div>
                <div class="time">{{ratingItem.rateTime | formatDate}}</div>
                <p class="text">
                  <i :class="{'icon-thumb_up':ratingItem.rateType === 0,'icon-thumb_down':ratingItem.rateType === 1}"></i>
                  {{ratingItem.text}}
                </p>
              </li>
            </ul>
          </div>
        </div>
      </div>
    </div>
  </transition>
</template>

<script>
import BScroll from 'better-scroll'
import cartcontrol from '../cartcontrol/cartcontrol'
import Vue from 'vue'
import split from '../split/split'
import ratingselect from '../ratingselect/ratingselect'
import {formatDate} from '../../common/js/date'

const POSITIVE = 0
const NEGATIVE = 1
const ALL = 2

export default {
  data () {
    return {
      showFlag: false,
      selectType: ALL,
      onlyContent: true,
      desc: {
        all: '全部',
        positive: '推荐',
        negative: '吐槽'
      }
    }
  },
  props: ['food'],
  methods: {
    show () {
      this.showFlag = true
      this.selectType = ALL
      this.onlyContent = true
      this.$nextTick(() => {
        if (!this.scroll) {
          this.scroll = new BScroll(this.$refs.food, {
            click: true
          })
        } else {
          this.scroll.refresh()
        }
      })
    },
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
    hide () {
      this.showFlag = false
    },
    addFirst (event) {
      if (!event._constructed) {
        return
      }
      this.$emit('add', event.target)
      Vue.set(this.food, 'count', 1)
    },
    toggleContent () {
      this.onlyContent = !this.onlyContent
      this.$nextTick(() => {
        this.scroll.refresh()
      })
    },
    selectRating (type) {
      this.selectType = type
      this.$nextTick(() => {
        this.scroll.refresh()
      })
    },
    addFood (target) {
      this.$emit('add', target)
    }
  },
  components: {
    cartcontrol, split, ratingselect
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
@import './css/food.styl'          
</style>
