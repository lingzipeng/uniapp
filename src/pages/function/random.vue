<template>
  <view class="container">
    <view class="number">{{ randomNumber }}</view>
    <button @click="generateRandomNumber">生成随机数字</button>
  </view>
</template>

<script setup lang="ts">
import { ref } from "vue";

const randomNumber = ref<number>(0);

const generateRandomNumber = () => {
  // 生成1到100之间的随机整数
  randomNumber.value = Math.floor(Math.random() * 100) + 1;
};
</script>

<style scoped>
.container {
  margin-top: 130px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.number {
  font-size: 24px;
  margin-bottom: 20px;
}

button {
  background-color: #007aff;
  color: #fff;
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}
</style>
