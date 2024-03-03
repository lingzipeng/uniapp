<template>
  <view>
    <text>你好</text>
    <uni-card is-full>
      <text class="uni-h6"
        >发送短信验证等待时间等场景======</text
      >
    </uni-card>
    <uni-section title="一般用法" type="line" padding>
      <uni-countdown :day="1" :hour="1" :minute="12" :second="40" />
    </uni-section>
  </view>
</template>

<script lang="ts"></script>

<style scoped></style>
