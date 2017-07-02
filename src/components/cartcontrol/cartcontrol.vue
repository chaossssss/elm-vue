<template>
  <div class="cartcontrol">
    <transition name="move">
      <div class="cart-decrease" @click.stop="decreaseCart" v-show="food.count > 0">
        <i class="inner icon-remove_circle_outline"></i>
      </div>
    </transition>
    <div class="inner cart-count" v-show="food.count > 0">{{food.count}}</div>
    <div class="cart-add icon-add_circle" @click.stop="addCart"></div>
  </div>
</template>

<script>
import Vue from 'vue'
export default {
  data () {
    return {

    }
  },
  props: {
    food: {
      type: Object
    }
  },
  created () {
    // console.log(this.food)
  },
  methods: {
    addCart (event) {
      if (!event._constructed) {
        return
      }
      console.log(event)
      if (!this.food.count) {
        Vue.set(this.food, 'count', 1)
        this.food.count = 1
      } else {
        this.food.count ++
      }
      this.$emit('cart.add', event.target)
    },
    decreaseCart (event) {
      if (!event._constructed) {
        return
      }
      if (this.food.count) {
        this.food.count --
      }
    }
  }
}
</script>

<style lang="stylus" rel="stylesheet/stylus" scoped>
@import './css/cartcontrol.styl'
</style>
