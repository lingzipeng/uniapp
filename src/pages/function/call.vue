<template>
  <view class="container">
    <view class="button" @click="simulateIncomingCall">模拟来电</view>
  </view>
</template>

<script setup>
import { defineComponent } from "vue";

const simulateIncomingCall = () => {
  uni.showModal({
    title: "模拟来电",
    content: "收到来电！",
    showCancel: false,
    confirmText: "接听",
    success(res) {
      if (res.confirm) {
        // 在这里可以添加接听电话后的逻辑
      }
    },
  });
};

defineComponent({
  setup() {
    return {
      simulateIncomingCall,
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
