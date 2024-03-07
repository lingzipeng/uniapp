<template>
  <view class="container">
    <textarea v-model="text" placeholder="请输入文字"></textarea>
    <view class="result">
      <view>中文字符数：{{ chineseCount }}</view>
      <view>英文字符数：{{ englishCount }}</view>
    </view>
  </view>
</template>

<script setup lang="ts">
import { ref, computed } from "vue";

const text = ref<string>("");
const chineseCount = computed(() => {
  const chineseRegex = /[\u4e00-\u9fa5]/gu;
  const chineseMatches = text.value.match(chineseRegex);
  return chineseMatches ? chineseMatches.length : 0;
});
const englishCount = computed(() => {
  const englishRegex = /[a-zA-Z]/gu;
  const englishMatches = text.value.match(englishRegex);
  return englishMatches ? englishMatches.length : 0;
});
</script>

<style lang="scss" scoped>
.container {
  padding: 20px;
}

textarea {
  width: 95%;
  height: 100px;
  border: 1px solid #ccc;
  border-radius: 4px;
  padding: 10px;
  margin-bottom: 20px;
}

.result {
  border: 1px solid #ccc;
  padding: 10px;
  border-radius: 4px;
}
</style>
