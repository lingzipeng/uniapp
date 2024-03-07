<template>
  <view class="container">
    <view class="dice" @click="rollDice">{{ diceValue }}</view>
  </view>
</template>

<script setup>
import { defineComponent, ref } from "vue";

const diceValue = ref(6);

const rollDice = () => {
  diceValue.value = Math.floor(Math.random() * 6) + 1; // 模拟掷骰子，生成1到6之间的随机数
};

defineComponent({
  setup() {
    return {
      diceValue,
      rollDice,
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

.dice {
  width: 100px;
  height: 100px;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 36px;
  background-color: #f0f0f0;
  border: 2px solid #333;
  border-radius: 10px;
  cursor: pointer;
}
</style>
