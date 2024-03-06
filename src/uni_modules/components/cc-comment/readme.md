# 环境

基于vue3.2和uni-ui开发; 
依赖版本参考如下:

```
{
    "dependencies": {
    "@dcloudio/uni-mp-weixin": "3.0.0-3090820231124001",
    "@dcloudio/uni-ui": "^1.4.28",
    "uni-ui": "^1.0.0",
    "vue": "^3.2.45"
  },
 "devDependencies": {
    "vite": "4.0.3"
  }
}
```

小程序调试基础库: 3.3.0

# 场景

即拿即用, 组件有详细注释内容, 方便二次开发; 
目前仅在小程序使用, 其他平台能否使用请评论留言反馈谢谢, 祝大家使用愉快.

# 附言

主要是插件市场没找到满意的, 诸多用着也不顺, 所以有了cc-comment, 如有Bug请留言或Email, 开源不易且用且珍惜, 感谢使用.
关于功能样式, 主打一个借鉴如下; 
参考[*小红书*]App的评论回复功能: 点击评论内容展示回复弹窗, 二级评论默认显示一条, 点击展开查看更多, 无评论收起功能.
参考[*什么值得买*]App的多层级回复评论title显示效果

# 功能

#### 已实现

* 无评论显示占位场景 √
* 显示评论数量(新增和删除动态更新数量) √
* 发起新评论 √
* 点击评论内容回复 √
* 回复一级评论 √
* 回复二级评论 √
* 折叠过多的二级评论 √
* 展开超长评论内容 √
* 不能回复自身评论 √
* 删除 √
* 仅可删除自身评论 √
* 可选三类删除模式 √
* 点赞 √
* 点赞大于100显示99+ √

#### 待实现

* 图片上传 ×

有其他需求的评论区留言

# :props 属性

| 属性名 | 说明  | 类型  |  默认值  |  必填  | 说明  |
| -------- | -------- | -------- |-------- |-------- |-------- |
| ref     | 实例     | Object     | -     | true     | |
| tableData     | 评论列表     | Array     | [ ]     | true     | |
| tableTotal     | 评论总数     | Number     | 0     | true     | |
| deleteMode     | 评论删除模式     | String     | all     | false     |  bind-当被删除的一级评论存在回复评论, 那么该评论内容变更显示为[当前评论内容已被移除] only-仅删除当前评论(后端删除相关联的回复评论, 否则总数显示不对) all-删除所有评论包括回复评论 |

# @event 事件

| 属性名 | 说明  | 参数  |  说明  |
| -------- | -------- | -------- |-------- |
| likeClick | 点赞事件 | {{params}, callback} | { params: 评论id }, callback回调函数, 请求后端接口后调用, 执行后续逻辑     |
| replyClick | 回复事件 | {{params}, callback} | { params: 评论参数 }, callback回调函数, 请求后端接口后调用, 执行后续逻辑     |
| deleteClick | 删除事件 | {{params, mode}, callback} | { params: 评论数组id, mode: 删除模式[all, bind, only] }, callback回调函数, 请求后端接口后调用, 执行后续逻辑   |

# $ref 实例可调用属性&事件

| 属性名 | 说明  | 回调参数  |  说明  |  平台差异说明  |
| -------- | -------- | -------- |-------- |-------- |
| newCommentFun     | 发起新评论     | -     | event     | -     |

# 数据说明

```javascript=
// 用户信息
type userInfoKeys = {

      id: number // 用户id
      user_name: string // 用户名
      user_avatar: string // 用户头像地址

}
// 评论表
type tableDataKeys = {

      id: number // 评论id
      parent_id: number // 父级评论id
      reply_id: number // 被回复人评论id
      reply_name: string // 被回复人名称
      user_name: string // 用户名
      user_avatar: string // 评论者头像地址
      user_content: string // 评论内容
      is_like: boolean // 是否点赞
      like_count: number // 点赞数统计
      create_time: string // 创建时间

}

```

# 使用方法
```javascript=
// 复制整页代码配合下载的插件, 可正常运作(提示: 注意uni-ui版本, 可能uni-ui不同版本的$变量不一致, 自行配置颜色即可)
<template>
  <CComment
    ref="ccRef"
    v-model:userInfo="userInfo"
    v-model:tableData="tableData"
    v-model:tableTotal="tableTotal"
    @likeFun="likeFun"
    @replyFun="replyFun"
    @deleteFun="deleteFun"
    :deleteMode="deleteMode"
  ></CComment>
  <view class="btn" @tap="openComment">发一条新评论</view>
</template>

<script setup>
import CComment from "@/components/bb-comment/index";
import { ref } from "vue";

// 唤起新评论弹框
let ccRef = ref(null);
function openComment() {
  ccRef.value.newCommentFun();
}

// 点赞回调事件
function likeFun({ params }, callback) {
  console.log("likeFun", params);
  // 当请求失败, 调用callback重置点赞效果;
  // Demo如下:
  // axios.post("http://xxx/like", { id: params }).then((res) => {
  //   if (res.code !== 0) {
  //     callback(res);
  //   }
  // });
}

// 评论回调事件
function replyFun({ params }, callback) {
  console.log("replyFun", params);
  // 当请求成功, 调用callback执行评论插入;
  // Demo如下:
  // axios.post("http://xxx/reply", { ...params }).then((res) => {
  //   if (res.code === 0) {
  //     callback(res);
  //   }
  // });
  const res = { id: Math.random() }; // 很重要的回参! 必须拿到后端返回评论id! 删除需要!
  setTimeout(() => callback(res), 500); // 目前为了展示效果, 直接执行callback
}

/** 删除回调事件
 * mode 删除模式
 * -- bind: 当被删除的一级评论存在回复评论, 那么该评论内容变更显示为[当前评论内容已被移除]
 * -- only: 仅删除当前评论(后端删除相关联的回复评论, 否则总数显示不对)
 * -- all : 删除所有评论包括回复评论
 */
const deleteMode = ref("all");
function deleteFun({ params, mode }, callback) {
  console.log("deleteFun", { params, mode });
  // 当请求成功, 调用callback执行评论删除;
  switch (deleteMode) {
    case "bind":
      // 逻辑: 调用接口进行评论内容修改 update
      setTimeout(() => callback(), 500); // 目前为了展示效果, 直接执行callback
      break;
    case "only":
      // 逻辑: 调用接口删除一个评论 delete
      setTimeout(() => callback(), 500); // 目前为了展示效果, 直接执行callback
      break;
    default:
      // all
      // 逻辑: 调用接口删除多个评论 [delete]
      // Demo如下:
      // axios.post("http://xxx/delete", { ids: params }).then((res) => {
      //   if (res.code === 0) {
      //     callback(res);
      //   }
      // });
      setTimeout(() => callback(), 500); // 目前为了展示效果, 直接执行callback
      break;
  }
}

// ----模拟数据------模拟数据------模拟数据----
// 作者信息(提示: 一般来自localstorage, 如果是实时获取的话, 那么获取到数据后再v-if显示评论组件)
let userInfo = ref({
  id: 120, // 评论id
  user_name: "ikun", // 用户名
  user_avatar:
    "https://pic1.zhimg.com/80/v2-a79071a705f55c5d88f6c74e6111fe84_720w.webp", // 用户头像
});
let tableTotal = ref(4); // 评论总数
let tableData = ref([
  {
    id: 120, // 评论id
    parent_id: null, // 父级评论id
    reply_id: null, // 被回复人评论id
    reply_name: null, // 被回复人名称
    user_name: "ikun", // 用户名
    user_avatar:
      "https://pic1.zhimg.com/80/v2-a79071a705f55c5d88f6c74e6111fe84_720w.webp", // 评论者头像地址
    user_content: "唱,跳,rap,篮球", // 评论内容
    is_like: false, // 是否点赞
    like_count: 120, // 点赞数统计
    create_time: "2024-01-01 09:16", // 创建时间
  },
  {
    id: 130,
    parent_id: 120, // 评论的父级id
    reply_id: 120, // 被回复评论id
    reply_name: "ikun", // 被回复人名称
    user_name: "小黑子", // 用户名
    user_avatar:
      "https://pic2.zhimg.com/80/v2-06eade66ec837713d765b1557bf20b25_720w.webp", // 评论者头像地址
    user_content: "姬霓太美", // 评论内容
    is_like: false, // 是否点赞
    like_count: 67, // 点赞数统计
    create_time: "2024-01-01 17:06", // 创建时间
  },
  {
    id: 140,
    parent_id: 120, // 评论的父级id
    reply_id: 130, // 被回复评论id
    reply_name: "小黑子", // 被回复人名称
    user_name: "守护宗主维护宗门", // 用户名
    user_avatar:
      "https://pic3.zhimg.com/80/v2-244696a62fa750b8570cf56bfaa5b26a_720w.webp", // 评论者头像地址
    user_content: "你露出鸡脚了", // 评论内容
    is_like: false, // 是否点赞
    like_count: 16, // 点赞数统计
    create_time: "2024-01-02 23:08", // 创建时间
  },
  {
    id: 150,
    parent_id: null, // 评论的父级id
    reply_id: null, // 被回复评论id
    reply_name: null, // 被回复人名称
    user_name: "音乐制作人", // 用户名
    user_avatar:
      "https://pic2.zhimg.com/80/v2-88ec6f8c6d3305122664dd18a28730e5_720w.webp", // 评论者头像地址
    user_content:
      "只因你太美baby 只因你太美baby 只因你实在是太美baby 只因你太美baby 迎面走来的你让我如此蠢蠢欲动 这种感觉我从未有 Cause I got a crush on you who you 你是我的 我是你的 谁 再多一眼看一眼就会爆炸 再近一点靠近点快被融化", // 评论内容
    is_like: true, // 是否点赞
    like_count: 8, // 点赞数统计
    create_time: "2024-01-08 00:45", // 创建时间
  },
]); // 评论表
</script>

<style lang="scss" scoped>
.btn {
  text-align: center;
  color: #fff;
  padding: 20rpx;
  margin: 20rpx;
  border-radius: 20rpx;
  background-color: #2979ff;
}
</style>

```
