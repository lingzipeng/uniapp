<template>
  <view>
    <view class="uni-margin-wrap">
      <swiper
        class="swiper"
        circular
        :indicator-dots="indicatorDots"
        :autoplay="autoplay"
        :interval="interval"
        :duration="duration"
      >
        <swiper-item>
          <image src="../static/pic/sc18.jpg" mode="widthFix" />
        </swiper-item>
        <swiper-item>
          <image src="../static/pic/sc19.jpg" mode="widthFix" />
        </swiper-item>
        <swiper-item>
          <image src="../static/pic/sc20.jpg" mode="widthFix" />
        </swiper-item>
      </swiper>
    </view>
  </view>
</template>

<script setup lang="ts">
import { ref } from "vue";
const indicatorDots = ref(true);
const autoplay = ref(true);
const interval = ref(2000);
const duration = ref(500);
</script>
<style lang="scss" scoped>
.uni-margin-wrap {
  position: absolute;
  right: 40rpx;
  width: 690rpx;
  width: 90%;
}
.swiper {
  height: 300rpx;
}
.swiper-item {
  display: block;
  height: 300rpx;
  line-height: 300rpx;
  text-align: center;
}
.swiper-list {
  margin-top: 40rpx;
  margin-bottom: 0;
}
.uni-common-mt {
  margin-top: 60rpx;
  position: relative;
}
.info {
  position: absolute;
  right: 20rpx;
}
.uni-padding-wrap {
  width: 550rpx;
  padding: 0 100rpx;
}
</style>
