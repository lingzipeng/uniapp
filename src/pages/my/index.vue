<template>
  <view class="content">
    <view class="content_image">
      <image src="../static/pic/sc18.jpg" mode="widthFix" />
    </view>
    <!-- 内容 -->
    <view class="content_botton">
      <view class="content_info" v-if="userinfo">
        <image src="../static/pic/sc18.jpg" mode="widthFix" />
        <view class="username"> 张三 </view>
        <view class="select">
          <view>微信号：12345</view>
          <view>二维码</view>
		  <uni-icons @click="erweima" type="forward" size="15"></uni-icons>
        </view>
      </view>
      <view v-else class="noLogin">
        <image
          src="../static/pic/sc18.jpg"
          mode="widthFix"
          @click="login"
        ></image>
        <view class="">点击图片登录</view>
      </view>
      <view class="other">
        <view class="other_addres" @click="addAdress">
          <view>服务</view>
          <uni-icons type="forward" size="15"></uni-icons>
        </view>
        <view class="other_addres ">
          <view>收藏</view>
		  <uni-icons type="forward" size="15"></uni-icons>
        </view>
		<view class="other_addres">
          <view>朋友圈</view>
		  <uni-icons type="forward" size="15"></uni-icons>
        </view>
		<view class="other_addres">
          <view>卡包</view>
		  <uni-icons type="forward" size="15"></uni-icons>
        </view>
		<view class="other_addres">
          <view>表情</view>
		  <uni-icons type="forward" size="15"></uni-icons>
        </view>
		<view class="other_our">
          <view>设置</view>
		  <uni-icons type="forward" size="15"></uni-icons>
        </view>
      </view>
    </view>
  </view>
</template>
<script setup lang="ts">
import { ref, onMounted } from "vue";

//二维码
const erweima = () => {
	console.log(1111)
}

let url = ref("");
let userinfo = ref([]);
let collectNum = ref(0);

onMounted(() => {
  userinfo.value = uni.getStorageSync("user");
  url.value = uni.getStorageSync("url");
  let collect = uni.getStorageSync("collect");
  collectNum.value = collect.length;
});

const login = () => {
  // Your login function here
};

// 添加地址
const addAdress = () => {
  uni.chooseAddress({
    success(res) {
      console.log(res);
      uni.setStorageSync("address", res);
    },
  });
};
</script>

<style lang="scss">
page {
  background-color: #eee;
}
.content_image {
  image {
    filter: blur(3rpx);
    width: 100%;
  }
  height: 300rpx;
  position: relative;
}
.content_botton {
  .noLogin {
    position: absolute;
    left: 45%;
    transform: translate(-50%);
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: space-around;
    margin-top: 15%;
    margin-left: 5%;
    border-radius: 20rpx;
    width: 90%;
    height: 250rpx;
    background-color: #fff;
    justify-content: center;
    align-items: center;
    position: relative;
    image {
      width: 100rpx;
      height: 100rpx;
    }
  }
  .content_info {
    position: relative;
    z-index: 999;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: space-around;
    margin-top: 15%;
    margin-left: 5%;
    border-radius: 20rpx;
    width: 90%;
    height: 250rpx;
    background-color: #fff;
    image {
      margin-top: -40rpx;
      width: 130rpx;
      height: 130rpx;
      border-radius: 50%;
    }
    .username {
    }
    .select {
      display: flex;
      justify-content: space-between;
      view {
        padding: 0 40rpx;
      }
    }
  }
  .my_order {
    width: 90%;
    margin-top: 10rpx;
    margin-left: 5%;
    height: 280rpx;
    border-radius: 20rpx;
    background-color: #fff;
    .my_order_title {
      border-radius: 20rpx;
      padding-left: 20rpx;
      line-height: 80rpx;
      height: 80rpx;
    }
    .my_order_item {
      padding: 4.5%;
      float: left;
      .my_order_content {
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: space-between;
        image {
          width: 80rpx;
          height: 50rpx;
        }
      }
    }
  }
}
.other {
  margin: 10rpx 40rpx;
  height: 580rpx;
  width: 90%;
  border-radius: 20rpx;
  background-color: #fff;
  .other_addres {
    height: 60rpx;
    padding: 20rpx 20rpx;
    border-bottom: 1rpx dashed gainsboro;
    display: flex;
    justify-content: space-between;
    image {
      width: 50rpx;
    }
  }
  .other_our {
    height: 60rpx;
    padding: 20rpx 20rpx;
    display: flex;
    justify-content: space-between;
    image {
      width: 50rpx;
    }
  }
}
</style>
