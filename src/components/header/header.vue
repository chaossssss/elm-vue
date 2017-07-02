<template>
  <div class="top-header">
    <div class="content-wrapper">
      <div class="avatar">
        <img width="64" height="64" :src="seller.avatar">
      </div>
      <div class="content">
        <div class="title">
          <span class="brand"></span>
          <span class="name">{{seller.name}}</span>
        </div>
        <div class="description">
          {{seller.description}}/{{seller.deliveryTime}}分钟送达
        </div>
        <div v-if="seller.supports" class="support">
          <span class="icon" :class="classMap[seller.supports[0].type]"></span>
          <span class="text">{{seller.supports[0].description}}</span>
        </div>
      </div>
      <div v-if="seller.supports" @click="showDetail" class="support-count">
        <span class="count">{{seller.supports.length}}个</span>
        <i class="icon-keyboard_arrow_right"></i>
      </div>
    </div>
    <div class="bulletin-wrapper">
      <span class="bulletin-title"></span>
      <span class="bulletin-text">{{seller.bulletin}}</span>
      <i class="icon-keyboard_arrow_right"></i>
    </div>
    <div class="background">
      <img :src="seller.avatar" width="100%" height="100%">
    </div>
    <transition name="fade">
      <div v-show="detailShow" class="detail">
        <div class="detail-wrapper clearFix">
          <div class="detail-main">
            <div class="name">{{seller.name}}</div>
            <div class="star-wrapper">
              <star :size="48" :score="seller.score"></star>
            </div>
            <div class="title">
              <div class="line"></div>
              <div class="text">优惠信息</div>
              <div class="line"></div>      
            </div>
            <div class="supports">
              <ul v-if="seller.supports">
                <li class="support-item" v-for="(item, index) in seller.supports">
                  <!-- <span class="icon" :class="classMap[seller.supports[index].type]"></span> -->
                  <icon :iconType="seller.supports[index].type" :iconSeries="2"></icon>
                  <span class="text">{{seller.supports[index].description}}</span>
                </li>
              </ul>
            </div>
            <div class="title">
              <div class="line"></div>
              <div class="text">商家公告</div>
              <div class="line"></div>  
            </div>
            <div class="bulletin">
              <div class="content">{{seller.bulletin}}</div>
            </div>
          </div>
        </div>
        <div class="detail-close">
          <i class="icon-close" @click="closeWindow"></i>
        </div>
      </div>
    </transition>
  </div>
</template>

<script>
import star from '../star/star'
import icon from '../icon/icon'
export default {
  data () {
    return {
      detailShow: false
    }
  },
  created () {
    this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee']
  },
  methods: {
    showDetail () {
      this.detailShow = true
    },
    closeWindow () {
      this.detailShow = false
    }
  },
  props: {
    seller: {
      type: Object
    }
  },
  components: {
    star, icon
  }
}
</script>

<style lang="stylus" rel="stylesheet/stylus" scoped>
@import "./css/header.styl"
</style>
