<template>
  <view class="c_total">评论 {{ props.tableTotal }}</view>
  <template v-if="dataList && dataList.length">
    <view class="c_comment" v-for="(item1, index1) in dataList" :key="item1.id">
      <!-- 一级评论 -->
      <CommonComp
        :data="item1"
        @likeClick="() => likeClick({ item1, index1 })"
        @replyClick="() => replyClick({ item1, index1 })"
        @deleteClick="() => deleteClick({ item1, index1 })"
      />
      <view
        class="children_item"
        v-if="item1.children && item1.children.length"
      >
        <!-- 二级评论 -->
        <CommonComp
          v-for="(item2, index2) in item1.childrenShow"
          :key="item2.id"
          :data="item2"
          :pData="item1"
          @likeClick="() => likeClick({ item1, index1, item2, index2 })"
          @replyClick="() => replyClick({ item1, index1, item2, index2 })"
          @deleteClick="() => deleteClick({ item1, index1, item2, index2 })"
        />
        <!-- 展开折叠的二级评论 -->
        <view
          class="expand_reply"
          v-if="expandTxtShow({ item1, index1 })"
          @tap="() => expandReplyFun({ item1, index1 })"
        >
          <span class="txt">
            展开{{ item1.children.length - item1.childrenShow.length }}条回复
          </span>
          <uni-icons type="down" size="24" color="#007aff"></uni-icons>
        </view>
      </view>
    </view>
  </template>
  <!-- 空盒子 -->
  <view class="empty_box" v-else>
    <uni-icons type="chatboxes" size="36" color="#c0c0c0"></uni-icons>
    <view>
      <span class="txt"> 这里是一片荒草地, </span>
      <span class="txt click" @click="() => newCommentFun()">说点什么...</span>
    </view>
  </view>
  <!-- 评论弹窗 -->
  <uni-popup ref="cPopupRef" type="bottom" @change="popChange">
    <view class="c_popup_box">
      <view class="reply_text">
        <template v-if="Object.keys(replyTemp).length">
          <span class="text_aid">回复给</span>
          <span class="text_main">{{
            replyTemp.item2
              ? replyTemp.item2.user_name
              : replyTemp.item1.user_name
          }}</span>
        </template>
        <span v-else class="text_main">发表新评论</span>
      </view>
      <view class="content">
        <view class="text_area">
          <uni-easyinput
            class="text_area"
            type="textarea"
            v-model="commentValue"
            :placeholder="commentPlaceholder"
            :focus="focus"
            trim
            autoHeight
            maxlength="300"
          ></uni-easyinput>
        </view>
        <view class="send_btn" @tap="() => sendClick()">发送</view>
      </view>
    </view>
  </uni-popup>
  <!-- 删除弹窗 -->
  <uni-popup ref="delPopupRef" type="dialog">
    <uni-popup-dialog
      mode="base"
      title=""
      content="确定删除这条评论吗?"
      :before-close="true"
      @close="delCloseFun"
      @confirm="delConfirmFun"
    ></uni-popup-dialog>
  </uni-popup>
</template>

<script setup>
import CommonComp from "./componets/common";
import { reactive, ref, watch, computed, defineExpose } from "vue";

const props = defineProps({
  /** 用户信息
   *    id: number // 用户id
   *    user_name: number // 用户名
   *    user_avatar: string // 用户头像地址
   */
  userInfo: {
    type: Object,
    default: () => {},
  },
  /** 评论列表
   *    id: number // 评论id
   *    parent_id: number // 父级评论id
   *    reply_id: number // 被回复人评论id
   *    reply_name: string // 被回复人名称
   *    user_name: string // 用户名
   *    user_avatar: string // 评论者头像地址
   *    user_content: string // 评论内容
   *    is_like: boolean // 是否点赞
   *    like_count: number // 点赞数统计
   *    create_time: string // 创建时间
   */
  tableData: {
    type: Array,
    default: () => [],
  },
  // 评论总数
  tableTotal: {
    type: Number,
    default: 0,
  },
  // 评论删除模式
  // bind - 当被删除的一级评论存在回复评论, 那么该评论内容变更显示为[当前评论内容已被移除]
  // only - 仅删除当前评论(后端删除相关联的回复评论, 否则总数显示不对)
  // all - 删除所有评论包括回复评论
  deleteMode: {
    type: String,
    default: "all",
  },
});
const emit = defineEmits([
  "update:tableTotal",
  "likeFun", // 点赞事件
  "replyFun", // 回复事件
  "deleteFun", // 删除事件
]);

// 渲染数据(前端的格式)
let dataList = ref([]);
watch(
  ()=>props.tableData,
  (newVal) => {
    if (newVal.length !== dataList.value.length) {
      let temp = props.tableData;
      dataList.value = treeTransForm(temp);
    }
  },
  { deep: true,  immediate: true  }
);

// 数据转换
function treeTransForm(data) {
  let newData = JSON.parse(JSON.stringify(data));
  let result = [];
  let map = {};
  newData.forEach((item, i) => {
    item.owner = item.id === props.userInfo.id; // 是否为所有者 所有者可以进行删除 不能回复
    map[item.id] = item;
  });
  newData.forEach((item) => {
    let parent = map[item.parent_id];
    if (parent) {
      (parent.children || (parent.children = [])).push(item); // 所有回复
      if (parent.children.length === 1) {
        (parent.childrenShow = []).push(item); // 显示的回复
      }
    } else {
      result.push(item);
    }
  });
  return result;
}

// 点赞
let setLike = (item) => {
  item.is_like = !item.is_like;
  item.like_count = item.is_like ? item.like_count + 1 : item.like_count - 1;
};
function likeClick({ item1, index1, item2, index2 }) {
  let item = item2 || item1;
  setLike(item);
  emit("likeFun", { params: item }, (res) => {
    // 请求后端失败, 重置点赞
    setLike(item);
  });
}

// 回复
let cPopupRef = ref(null); // 弹窗实例
let replyTemp = reactive({}); // 临时数据
function replyClick({ item1, index1, item2, index2 }) {
  replyTemp = JSON.parse(JSON.stringify({ item1, index1, item2, index2 }));
  cPopupRef.value.open();
}

// 发起新评论
let isNewComment = ref(false); // 是否为新评论
defineExpose({ newCommentFun });
function newCommentFun() {
  isNewComment.value = true;
  cPopupRef.value.open();
}

// 评论弹窗
let focus = ref(false);
function popChange(e) {
  // 关闭弹窗
  if (!e.show) {
    commentValue.value = ""; // 清空输入框值
    replyTemp = {}; // 清空被回复人信息
    isNewComment.value = false; // 恢复是否为新评论默认值
  }
  focus.value = e.show;
}
let commentValue = ref(""); // 输入框值
let commentPlaceholder = ref("说点什么..."); // 输入框占位符

// 发送评论
function sendClick({ item1, index1, item2, index2 } = replyTemp) {
  let item = item2 || item1;
  let params = {};
  // 新评论
  if (isNewComment.value) {
    params = {
      id: Math.random(), // 评论id
      parent_id: null, // 父级评论id
      reply_id: null, // 被回复评论id
      reply_name: null, // 被回复人名称
    };
  } else {
    // 回复评论
    params = {
      id: Math.random(), // 评论id
      parent_id: item?.parent_id ?? item.id, // 父级评论id
      reply_id: item.id, // 被回复评论id
      reply_name: item.user_name, // 被回复人名称
    };
  }
  params = {
    ...params,
    user_name: props.userInfo.user_name, // 用户名
    user_avatar: props.userInfo.user_avatar, //  用户头像地址
    user_content: commentValue.value, //  用户评论内容
    is_like: false, // 是否点赞
    like_count: 0, // 点赞数统计
    create_time: "刚刚", // 创建时间
    owner: true, // 是否为所有者 所有者可以进行删除 管理员默认true
  };

  uni.showLoading({
    title: "正在发送",
    mask: true,
  });
  emit("replyFun", { params }, (res) => {
    uni.hideLoading();
    // 拿到后端返回的id赋值, 因为删除要用到id
    params = { ...params, id: res.id };
    // 新评论
    if (isNewComment.value) {
      dataList.value.push(params);
    } else {
      // 回复
      let c_data = dataList.value[index1];
      (c_data.children || (c_data.children = [])).push(params);
      // 如果已展开所有回复, 那么此时插入children长度会大于childrenShow长度1, 所以就直接展开显示即可
      if (
        c_data.children.length ===
        (c_data.childrenShow || (c_data.childrenShow = [])).length + 1
      ) {
        c_data.childrenShow.push(params);
      }
    }
    emit("update:tableTotal", props.tableTotal + 1);
    cPopupRef.value.close();
  });
}

// 删除
const delPopupRef = ref(null);
let delTemp = reactive({}); // 临时数据
function deleteClick({ item1, index1, item2, index2 }) {
  delTemp = JSON.parse(JSON.stringify({ item1, index1, item2, index2 }));
  delPopupRef.value.open();
}
// 关闭删除弹窗
function delCloseFun() {
  delTemp = {};
  delPopupRef.value.close();
}
// 确定删除
function delConfirmFun({ item1, index1, item2, index2 } = delTemp) {
  let c_data = dataList.value[index1];
  uni.showLoading({
    title: "正在删除",
    mask: true,
  });
  // 删除二级评论
  if (index2 >= 0) {
    emit("deleteFun", { params: [c_data.children[index2].id] }, (res) => {
      uni.hideLoading();
      emit("update:tableTotal", props.tableTotal - 1);
      c_data.children.splice(index2, 1);
      c_data.childrenShow.splice(index2, 1);
    });
  } else {
    // 删除一级评论
    if (c_data.children && c_data.children.length) {
      // 如果一级评论包含回复评论
      switch (props.deleteMode) {
        case "bind":
          // 一级评论内容展示修改为: 当前评论内容已被移除
          c_data.user_content = "当前评论内容已被移除";
          break;
        case "only":
          // 后端自行根据删除的一级评论id, 查找关联的子评论进行删除
          emit(
            "deleteFun",
            {
              mode: props.deleteMode,
              params: [c_data.id],
            },
            (res) => {
              uni.hideLoading();
              emit(
                "update:tableTotal",
                props.tableTotal - c_data.children.length + 1
              );
              dataList.value.splice(index1, 1);
            }
          );
          break;
        default:
          // all
          // 收集子评论id, 提交给后端统一删除
          let delIdArr = [c_data.id];
          c_data.children.forEach((_, i) => {
            delIdArr.push(_.id);
          });
          emit(
            "deleteFun",
            { params: delIdArr, mode: props.deleteMode },
            (res) => {
              uni.hideLoading();
              emit(
                "update:tableTotal",
                props.tableTotal - c_data.children.length + 1
              );
              dataList.value.splice(index1, 1);
            }
          );
          break;
      }
    } else {
      // 一级评论无回复, 直接删除
      emit("deleteFun", { params: [c_data.id] }, (res) => {
        uni.hideLoading();
        emit("update:tableTotal", props.tableTotal - 1);
        dataList.value.splice(index1, 1);
      });
    }
  }
  delCloseFun();
}

// 更多评论文字显示
function expandTxtShow({ item1, index1 }) {
  return (
    item1.childrenShow &&
    item1.childrenShow.length &&
    item1.children.length - item1.childrenShow.length
  );
}
// 展开更多评论
function expandReplyFun({ item1, index1 }) {
  let csLen = dataList.value[index1].childrenShow.length;
  dataList.value[index1].childrenShow.push(
    ...dataList.value[index1].children.slice(csLen, csLen + 6) // 截取5条评论
  );
}
</script>

<style lang="scss" scoped>
////////////////////////
.center {
  display: flex;
  align-items: center;
}
////////////////////////
.c_total {
  padding: 20rpx 30rpx 0 30rpx;
  font-size: 28rpx;
}
.empty_box {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  padding: 150rpx 10rpx;
  font-size: 28rpx;
  .txt {
    color: $uni-text-color-disable;
  }
  .click {
    color: $uni-color-primary;
  }
}
.c_comment {
  padding: 20rpx 30rpx;
  font-size: 28rpx;

  .children_item {
    padding: 20rpx 30rpx;
    margin-top: 10rpx;
    margin-left: 80rpx;
    background-color: $uni-bg-color-grey;
    .expand_reply {
      margin-top: 10rpx;
      margin-left: 80rpx;
      .txt {
        font-weight: 600;
        color: $uni-color-primary;
      }
    }
  }
}
.c_popup_box {
  background-color: #fff;
  .reply_text {
    @extend .center;
    padding: 20rpx 20rpx 0 20rpx;
    font-size: 26rpx;
    .text_aid {
      color: $uni-text-color-grey;
      margin-right: 5rpx;
    }
    .text_main {
    }
  }
  .content {
    @extend .center;
    .text_area {
      flex: 1;
      padding: 20rpx;
    }
    .send_btn {
      @extend .center;
      justify-content: center;
      width: 120rpx;
      height: 60rpx;
      border-radius: 20rpx;
      font-size: 28rpx;
      color: #fff;
      background-color: $uni-color-primary;
      margin-right: 20rpx;
    }
  }
}
</style>
