<template>
  <view class="baseInfo">
    <xnw-from
      :config="baseInfo"
      wxConfig="baseInfo"
      ref="baseInfo"
    />
    <view class="btnGroup fix">
      <view
        class="btn primary"
        @click="nextStep"
      >
        下一步
      </view>
    </view>
  </view>
</template>
<script>
export default {
  props: {
    baseInfo: {
      type: Object,
      default: () => ({ data: [{}] })
    }
  },
  data () {
    return {
      baseInfos: {}
    }
  },
  created () {
    this.baseInfos = this.baseInfo
    // #ifdef MP-WEIXIN
    this.baseInfos = this.globalData.wxPage.baseInfo
    console.log(this.baseInfos)
    // #endif
  },
  methods: {
    hasError (key, type, action) {
      let target = this.baseInfo.data.find(item => item.key === key)
      // #ifdef MP-WEIXIN
      target = this.globalData.wxPage.baseInfo.data.find(item => item.key === key)
      // #endif
      return target[type][action]('', target)
    },
    nextStep () {
      let baseInfo = this.$refs.baseInfo.getFromData()
      let hasError = []
      Object.keys(baseInfo).forEach(key => {
        switch (key) {
          case 'name':
          case 'duration':
            hasError.push(this.hasError(key, 'input', 'input'))
            break
          case 'channel':
            hasError.push(this.hasError(key, 'checkbox', 'change'))
            break
        }
      })
      if (hasError.includes(false)) return
      this.$emit('nextStep', baseInfo)
    }
  }
}
</script>
<style lang="scss" scoped>
.baseInfo {
  margin: 0 30rpx;
  /deep/ .form {
    .checkbox .label {
      width: 50%;
      display: inline-block;
      line-height: 3;
      text-align: center;
    }
  }
}
</style>
