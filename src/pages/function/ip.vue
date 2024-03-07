<template>
  <view class="container">
    <view class="input-container">
      <input v-model="ip" type="text" placeholder="请输入IP地址" />
      <button @click="queryIP">查询</button>
    </view>
    <view v-if="result" class="result">
      <view>IP地址：{{ result.ip }}</view>
      <view>国家：{{ result.country }}</view>
      <view>省份：{{ result.region }}</view>
      <view>城市：{{ result.city }}</view>
    </view>
  </view>
</template>

<script setup lang="ts">
import { ref } from "vue";

const ip = ref<string>("");
const result = ref<any>(null);

const queryIP = async () => {
  if (!ip.value) {
    uni.showToast({
      title: "请输入IP地址",
      icon: "none",
    });
    return;
  }
  try {
    const response = await uni.request({
      url: `https://api.ip.sb/geoip/${ip.value}`,
      method: "GET",
    });
    if (response.statusCode === 200) {
      result.value = response.data;
    } else {
      uni.showToast({
        title: "查询失败，请重试",
        icon: "none",
      });
    }
  } catch (error) {
    console.error("Error:", error);
    uni.showToast({
      title: "查询失败，请重试",
      icon: "none",
    });
  }
};
</script>

<style scoped>
.container {
  padding: 20px;
}

.input-container {
  display: flex;
  align-items: center;
  margin-bottom: 20px;
}

input {
  flex: 1;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

button {
  margin-left: 10px;
  padding: 10px 20px;
  background-color: #007aff;
  color: #fff;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.result {
  margin-top: 20px;
  border: 1px solid #ccc;
  padding: 10px;
  border-radius: 4px;
}
</style>
