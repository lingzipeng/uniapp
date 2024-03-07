<template>
  <view class="container">
    <view class="content">
      <text class="title">今天吃什么？</text>
    </view>
    <text class="food">{{ randomFood }}</text>
    <view class="button-container">
      <button @click="generateFood">换一个</button>
    </view>
  </view>
</template>

<script setup>
import { ref } from "vue";

const foods = [
  "炒饭",
  "牛肉面",
  "麻辣烫",
  "披萨",
  "火锅",
  "寿司",
  "汉堡",
  "烤肉",
  "拉面",
  "水饺",
  "饺子",
  "炸鸡",
  "粥",
  "炒面",
  "煎饼果子",
];

const randomFood = ref("");

const generateFood = () => {
  const randomIndex = Math.floor(Math.random() * foods.length);
  randomFood.value = foods[randomIndex];
};
</script>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
}

.content {
  margin-bottom: 30px;
}

.title {
  font-size: 24px;
  font-weight: bold;
  margin-bottom: 10px;
}

.food {
  font-size: 20px;
  color: #007aff;
}

.button-container {
    margin-top: 30px;
  display: flex;
}

button {
  padding: 10px 20px;
  margin: 0 10px;
  border: none;
  border-radius: 5px;
  background-color: #007aff;
  color: white;
  cursor: pointer;
}

button:hover {
  background-color: #0056b3;
}
</style>
