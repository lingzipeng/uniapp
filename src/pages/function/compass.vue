<template>
  <view class="compass-container">
    <view class="compass" ref="compass">
      <view class="arrow" :style="arrowStyle"></view>
    </view>
  </view>
</template>

<script setup lang="ts">
import { ref, onMounted, onBeforeUnmount, computed } from "vue";

const rotation = ref<number>(0);

const arrowStyle = computed(() => ({
  transform: `rotate(${rotation.value}deg)`,
}));

const handleOrientation = (event: DeviceOrientationEvent) => {
  if (event.webkitCompassHeading) {
    // iOS设备
    rotation.value = event.webkitCompassHeading;
  } else if (event.alpha) {
    // Android设备
    rotation.value = 360 - event.alpha;
  }
};

onMounted(() => {
  // 监听设备方向变化
  window.addEventListener("deviceorientation", handleOrientation);
});

onBeforeUnmount(() => {
  // 移除事件监听
  window.removeEventListener("deviceorientation", handleOrientation);
});
</script>

<style scoped>
.compass-container {
  width: 100vw;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}

.compass {
  width: 200px;
  height: 200px;
  border: 2px solid black;
  border-radius: 50%;
  position: relative;
}

.arrow {
  width: 2px;
  height: 100px;
  background-color: red;
  position: absolute;
  left: 50%;
  top: 50%;
  transform-origin: bottom center;
}
</style>
