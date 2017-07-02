<template>
  <div class="icon-wrapper">
    <span :class="[classMap[iconType], iconStyle]"></span>
  </div>
</template>

<script>
export default {
  data () {
    return {

    }
  },
  created () {
    this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee']
  },
  props: {
    iconType: {
      type: Number
    },
    iconSeries: {
      type: Number
    }
  },
  computed: {
    iconStyle () {
      return 'icon' + this.iconSeries
    }
  }
}
</script>

<style lang="stylus" rel="stylesheet/stylus">
@import "./css/icon.styl"
</style>
