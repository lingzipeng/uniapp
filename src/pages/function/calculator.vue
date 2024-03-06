<template>
  <div class="calculator">
    <div class="display">{{ display }}</div>
    <div class="buttons">
      <div v-for="button in buttons" :key="button" @click="handleButtonClick(button)">{{ button }}</div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';

const display = ref('0');
const buttons = ['7', '8', '9', '+', '4', '5', '6', '-', '1', '2', '3', '*', 'C', '0', '=', '/'];

const handleButtonClick = (button: string) => {
  if (button === 'C') {
    clearDisplay();
  } else if (button === '=') {
    calculate();
  } else {
    updateDisplay(button);
  }
};

const clearDisplay = () => {
  display.value = '0';
};

const updateDisplay = (button: string) => {
  if (display.value === '0') {
    display.value = button;
  } else {
    display.value += button;
  }
};

const calculate = () => {
  try {
    const result = eval(display.value);
    if (typeof result !== 'number' || isNaN(result)) {
      throw new Error('Invalid expression');
    }
    display.value = result.toString();
  } catch (error) {
    display.value = 'Error';
  }
};
</script>

<style scoped>
.calculator {
  width: 300px;
  margin: 0 auto;
  margin-top: 80px;
}

.display {
  font-size: 24px;
  padding: 10px;
  border: 1px solid #ccc;
  text-align: right;
}

.buttons {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 5px;
}

.buttons div {
  font-size: 20px;
  padding: 15px;
  background-color: #f0f0f0;
  text-align: center;
  cursor: pointer;
}

.buttons div:hover {
  background-color: #ccc;
}
</style>
