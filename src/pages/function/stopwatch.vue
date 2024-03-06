<template>
  <view>
    <button style="margin-top: 30px;" @click="showInputDialog">时间输入</button>
    <uni-section title="开始倒计时" type="line" padding class="stopwatch">
      <uni-countdown
        :showDay="false"
        :showHour="false"
        :start="start"
        :minute="testMinute"
        :second="testSecond"
      />
    </uni-section>
    <text>时间{{ testMinute }}分{{ testSecond }}秒 </text>
    <button @click="tostart">start</button>
  </view>
</template>
<script setup lang="ts">
import { ref, onMounted, onUnmounted } from "vue";

//保存时间
const testMinute = ref<number>(1);
const testSecond = ref<number>(0);
const start = ref<boolean>(false);

const tostart = () => {
  start.value = !start.value;
};

const showInputDialog = () => {
  uni.showModal({
    title: '请完成数据填写',
    content: '',
    editable: true, // 是否显示输入框
    placeholderText: '请输入时间(分钟)', // 输入框提示内容
    confirmText: '确认',
    cancelText: '取消',
    success: (res: { confirm: boolean; content: number }) => {
      if (res.confirm) {
        testMinute.value = res.content;
        start.value = true;
      }
    }
  });
};

onMounted(() => {
});

onUnmounted(() => {
  // 清理工作
});
</script>

<style lang="scss" scoped>
.stopwatch {
  margin-top: 150px;
  display: flex;
  justify-content: center;
}
</style>
