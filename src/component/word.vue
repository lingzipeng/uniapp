<template>
  <uni-card>
    <uni-section title="日常生活" type="line">
      <uni-grid :column="3" :highlight="true">
        <uni-grid-item v-for="func in funcs" :key="func.id">
          <navigator
            :url="`/pages/function/${func.id}`"
            hover-class="navigator-hover"
          >
            <view class="grid-item-box" style="background-color: #fff">
              <uni-icons :type="func.icon" :size="30" color="#777" />
              <text class="text">{{ func.function }}</text>
            </view>
          </navigator>
        </uni-grid-item>
      </uni-grid>
    </uni-section>
  </uni-card>
</template>

<script setup lang="ts">
import { reactive } from "vue";
const funcs = reactive([
  { id: "article", function: "文章评论", icon: "chatbubble" },
  { id: "calculator", function: "计算器", icon: "image" },
  { id: "calendar", function: "日历" ,icon: "calendar"},
  { id: "stopwatch", function: "倒计时" ,icon: "loop"},
  { id: "call", function: "模拟来电" ,icon: "phone-filled"},
  { id: "shock", function: "手机震动" ,icon: "fire-filled"},
]);
</script>

<style lang="scss" scoped>
.image {
  width: 25px;
  height: 25px;
}

.example-body {
  /* #ifndef APP-NVUE */
  /* #endif */
}

.grid-dynamic-box {
  margin-bottom: 15px;
}

.grid-item-box {
  flex: 1;
  /* #ifndef APP-NVUE */
  display: flex;
  /* #endif */
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 15px 0;
}

.grid-item-box-row {
  flex: 1;
  /* #ifndef APP-NVUE */
  display: flex;
  /* #endif */
  flex-direction: row;
  align-items: center;
  justify-content: center;
  padding: 15px 0;
}

.grid-dot {
  position: absolute;
  top: 5px;
  right: 15px;
}

.swiper {
  height: 420px;
}

/* #ifdef H5 */
@media screen and (min-width: 768px) and (max-width: 1425px) {
  .swiper {
    height: 630px;
  }
}

@media screen and (min-width: 1425px) {
  .swiper {
    height: 830px;
  }
}

/* #endif */
</style>
