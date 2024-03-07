<template>
  <view class="container">
    <view v-show="showKarma">功德+1</view>
    <text class="title">电子木鱼</text>
    <view class="woodfish" @click="beatWoodfish">
      <text>{{ count }}</text>
    </view>
    <button class="del" @click="del">清空</button>
  </view>
</template>

<script setup>
import { ref } from "vue";

const count = ref(0);
const showKarma = ref(false);

const beatWoodfish = () => {
  // 更新点击次数
  count.value += 1;

  // 显示功德+1字样
  showKarma.value = true;
  setTimeout(() => {
    showKarma.value = false;
  }, 1000); 
};

const del = () => {
    count.value = 0;
}
</script>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
}

.title {
  font-size: 24px;
  font-weight: bold;
  margin-bottom: 20px;
  margin-top:30px;
}

.woodfish {
  width: 100px;
  height: 100px;
  background-color: #f0f0f0;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
}

.woodfish text {
  font-size: 20px;
}

.karma {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  font-size: 20px;
  color: #007aff;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: #fff;
  padding: 10px 20px;
  border-radius: 5px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
  z-index: 999;
  opacity: 0;
  transition: opacity 0.5s ease;
}

.karma.show {
  opacity: 1;
}
.del{
    margin-top:30px;
}
</style>
