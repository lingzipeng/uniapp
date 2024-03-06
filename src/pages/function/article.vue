<template>
  <scroll-view scroll-y>
    <view class="container">
      <!--卡片-->

      <uni-card padding="0" spacing="0">
        <template v-slot:cover>
          <view class="custom-cover">
            <image class="cover-image" mode="aspectFill" :src="cover"> </image>
            <view class="cover-content">
              <text class="uni-subtitle uni-white">今日猫猫分享</text>
            </view>
          </view>
        </template>
      </uni-card>
      <!--  评价 -->
      <view class="evaluate">
        <view class="evaluateCenter">
          <view class="evaluateItem" v-for="(item, index) in list" :key="index">
            <view class="evaluateTip dfsb">
              <view class="right">
                <view class="righttip">{{ item.name }}</view>
                <view class="bottom dfsb">
                  <view class="star">
                    <text>{{ item.score }}分</text>
                    <uni-rate
                      size="12"
                      activeColor="#fd9130"
                      :is-fill="false"
                      readonly
                      disabledColor=""
                      allow-half
                      :value="item.score || 3.5"
                    />
                  </view>
                  <view class="time">{{ item.time }}</view>
                </view>
              </view>
            </view>
            <view class="evaluateText" :class="{ lineclamp3: item.contentAll }">
              {{ item.content }}
            </view>
            <view class="" v-if="item.isMore">
              <view
                class="rightText"
                v-if="item.contentAll"
                @click="changeAllFun(item, index)"
                >全部</view
              >
              <view class="rightText" v-else @click="changeAllFun(item, index)"
                >收起</view
              >
            </view>
            <view class="evaluateListImg">
              <view
                class="evaluateListImgItem"
                v-for="(itm, ind) in item.imageList"
                :key="ind"
              >
              </view>
            </view>
            <view class="bottomImg"> </view>
          </view>
        </view>
      </view>
    </view>
  </scroll-view>
  <view style="height: 30px"> </view>
  <!-- 表单 -->
  <view class="input">
    <uni-easyinput
      v-model="newItem.content"
      :styles="styles"
      :placeholderStyle="placeholderStyle"
      placeholder="请输入内容"
      @input="input"
    ></uni-easyinput>
    <button type="submit" @click.prevent="addItem">评论</button>
  </view>
</template>

<script setup lang="ts">
import { ref, onMounted } from "vue";
//图片地址
const cover = "https://qiniu-web-assets.dcloud.net.cn/unidoc/zh/shuijiao.jpg";
interface ListItem {
  name: string;
  score: number;
  time: string;
  content: string;
  isMore: boolean;
  contentAll: boolean;
}

const list = ref<ListItem[]>([
  {
    name: "马保国",
    score: 4.5,
    time: "2021.06.06",
    content:
      "大美湛江！！！服务好，景色好，山河锦绣，树木茂盛，空气清新，鸟语花香，人杰地灵服务好，景色好，山河锦绣，树木茂盛，空气清新，鸟语花香，人杰地灵服务好，景色好，山河锦绣，树木茂盛，空气清新，鸟语花香，人杰地灵服务好，景色好，山河锦绣，树木茂盛，空气清新，鸟语花香，人杰地灵服务好，景色好，山河锦绣，树木茂盛，空气清新，鸟语花香，人杰地灵",
    isMore: false,
    contentAll: false,
  },
]);

const newItem = ref({
  name: "坤哥",
  score: 2.5,
  time: "2024.01.01",
  content: "",
});

const addItem = () => {
  list.value.push({ ...newItem.value });
  newItem.value.content = "";
  getList();
};

onMounted(() => {
  getList();
});

function getList() {
  list.value.forEach((item) => {
    if (item.content.length > 60) {
      item.isMore = true;
      item.contentAll = true;
    } else {
      item.isMore = false;
      item.contentAll = false;
    }
  });
}

function changeAllFun(item: ListItem, index: number) {
  const updatedList = JSON.parse(JSON.stringify(list.value));
  updatedList.forEach((elem, ind) => {
    if (index === ind) {
      elem.contentAll = !elem.contentAll;
    }
  });
  list.value = updatedList;
}
</script>

<style lang="scss" scoped>
.container {
  width: 100vw;
  font-size: 12px;
  min-height: 100vh;
  display: inline-block;
  color: #1d1d1d;
  position: relative;
  background: #f1f1f1;
}
// 游客评价
.evaluate .evaluateCenter .evaluateItem {
  display: inline-block;
}
.rightText {
  color: #4399fc;
  text-align: right;
}
.evaluate {
  .tip {
    padding: 15px;
    .left {
      font-size: 14px;
      font-weight: 500;
      color: #0c1317;
    }
    .right {
      color: #0f9efa;
      font-weight: bold;
      image {
        margin-left: 6px;
        width: 16px;
        height: 16px;
      }
    }
  }
  .evaluateCenter {
    overflow-x: auto;
    .evaluateItem {
      background-color: #ffffff;
      padding: 15px;
      border-radius: 10px;
      margin: 10px 15px 0 15px;
      .evaluateTip {
        .img {
          width: 42px;
          height: 42px;
          display: table;
          image {
            border-radius: 50%;
          }
        }
        .right {
          width: 100%;
          padding-left: 15px;
          .righttip {
            font-size: 13px;
            font-family: PingFang SC;
            font-weight: bold;
            color: #0c1317;
            padding-bottom: 5px;
          }
          .bottom {
            .star {
              display: flex;
              color: #fd9130;
              text {
                padding-right: 4px;
                font-size: 11px;
                color: #0f9efa;
                font-weight: bold;
              }
            }
            .time {
              color: #969899;
            }
          }
        }
      }
      .evaluateText {
        margin: 10px 0 0 0;
        letter-spacing: 0.5px;
        line-height: 20px;
      }
      .lineclamp3 {
        overflow: hidden;
        text-overflow: ellipsis;
        display: -webkit-box;
        -webkit-line-clamp: 3;
        -webkit-box-orient: vertical;
        white-space: normal;
      }
      .evaluateListImg {
        width: 100%;
        .evaluateListImgItem {
          width: 66px;
          height: 66px;
          float: left;
          margin-right: 6px;
          image {
            border-radius: 5px;
          }
        }
      }
      .bottomImg {
        display: flex;
        width: 100%;
        padding-top: 15px;
        image {
          width: 18px;
          height: 18px;
          border-radius: 50%;
        }
        text {
          padding-left: 6px;
          font-size: 14px;
          color: #787b7c;
          font-weight: bold;
        }
      }
    }
  }
}
.input {
  position: fixed;
  bottom: 0;
  left: 0;
  width: 100%;
  display: flex;
  align-items: center;
}
</style>
