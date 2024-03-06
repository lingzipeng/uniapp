<template>
  <view class="container">
    <view class="button" @click="vibrate">点击震动</view>
  </view>
</template>

<script setup lang="ts">
import { defineComponent } from "vue";

const vibrate = () => {
  uni.vibrateLong(); // 或 uni.vibrateShort();
};

defineComponent({
  setup() {
    return {
      vibrate,
    };
  },
});
</script>

<style scoped>
.container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

.button {
  padding: 15px 30px;
  background-color: #007bff;
  color: #fff;
  border-radius: 5px;
  cursor: pointer;
}
</style>
