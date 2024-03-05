<template>
  <uni-card>
    <uni-section title="图片处理" type="line">
      <uni-grid :column="3" :highlight="true">
        <uni-grid-item v-for="(item, index) in 6" :index="index" :key="index">
          <view class="grid-item-box" style="background-color: #fff">
            <uni-icons type="image" :size="30" color="#777" />
            <text class="text">信息</text>
          </view>
        </uni-grid-item>
      </uni-grid>
    </uni-section>
  </uni-card>
</template>

<script setup lang="ts"></script>

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
