<template>
  <view style="background-color: #f7f7f7">
    <uni-notice-bar
      single
      scrollable
      color="#2979FF"
      background-color="#EAF2FF"
      text="uni-app是一个使用Vue.js开发所有前端应用的框架，开发者编写一套代码，可发布到iOS、Android、Web（响应式）、以及各种小程序、快应用等多个平台。"
    />
    <view class="uni-margin-wrap">
      <uni-card :is-shadow="false" is-full>
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
      </uni-card>
    </view>
    <view>
      <word />
      <pic />
      <compute />
    </view>
  </view>
</template>

<script setup lang="ts">
import { ref } from "vue";
import word from "@/component/word.vue";
import pic from "@/component/pic.vue";
import compute from "@/component/compute.vue";
const indicatorDots = ref(true);
const autoplay = ref(true);
const interval = ref(2000);
const duration = ref(500);
</script>
<style lang="scss" scoped>
.uni-margin-wrap {
  width: 690rpx;
  width: 100%;
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
