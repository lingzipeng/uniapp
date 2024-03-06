<template>
    <view>
        <view class="h_comment" v-for="(item, index) in  commentList " :key="item.id">



            <!-- 一级评论 => 列表 => -->
            <view class="h_left">
                <up-avatar size="64rpx" :src="item.url"></up-avatar>
            </view>

            <view class="h_right">
                <!-- 用户信息 -->
                <view class="h_info">
                    <view class="name" :style="{ color: themeColor }">{{ item.name }}</view>
                    <view class="like" :style="{ color: item.isLike ? themeColor : '#9a9a9a' }">
                        <view class="num" :style="{ color: item.isLike ? themeColor : '#9a9a9a' }">{{ item.likeNum }}</view>
                        <u-icon v-if="!item.isLike" name="thumb-up" :size="30" color="#9a9a9a"
                            @click="getLike(index)"></u-icon>
                        <u-icon v-if="item.isLike" name="thumb-up-fill" :color="themeColor" :size="30"
                            @click="getLike(index)"></u-icon>
                    </view>
                </view>

                <!-- 一级评论 => 文本 -->
                <view class="h_content">{{ item.contentText }}</view>

                <!-- 一级评论 => 日期、回复 -->
                <view class="h_date">
                    {{ item.date }}
                    <view class="h_revert" :style="{ color: themeColor }" @tap="showRevertPop(index, item, null, null)">
                        回复
                    </view>
                    <view class="h_remove" v-if="item.isMyComment" @tap="removeRevertModal(index, item, null, null)">
                        删除
                    </view>
                </view>

                <!-- 二级评论 -->
                <view class="h_revert_list">

                    <!-- 二级评论 => 列表 -->
                    <view class="item_s" v-for="( item_s, j ) in  item.replyList " :key="item_s.index">
                        <view class="user-info">
                            <up-avatar size="40rpx" :src="item_s.url"></up-avatar>
                            <view class="username">
                                <view class="left-name">
                                    {{ item_s.name }}
                                </view>

                                <u-icon v-if="item_s.to_user_id" name="play-right-fill" color="#868686" size="10"></u-icon>
                                <view class="right-name">
                                    {{ item_s.to_user_name }}
                                </view>
                            </view>
                            <view class="like" :style="{ color: item_s.isLike ? themeColor : '#9a9a9a' }">
                                <view class="num" :style="{ color: item_s.isLike ? themeColor : '#9a9a9a' }">{{
                                    item_s.likeNum }}</view>
                                <u-icon v-if="!item_s.isLike" name="thumb-up" :size="30" color="#9a9a9a"
                                    @click="getLike(index, j)"></u-icon>
                                <u-icon v-if="item_s.isLike" name="thumb-up-fill" :color="themeColor" :size="30"
                                    @click="getLike(index, j)"></u-icon>
                            </view>
                        </view>
                        <view class="text">{{ item_s.contentStr }}</view>
                        <view class="date">
                            {{ item_s.date }}
                            <view class="revert" :style="{ color: themeColor }"
                                @tap="showRevertPop(index, item, j, item_s)">回复</view>
                            <view class="h_remove" v-if="item_s.user_isMyComment" @tap="removeRevertModal(index, item, j, item_s)">
                                删除
                            </view>
                        </view>
                    </view>


                    <!-- loading -->
                    <u-loading-icon class="p-t-20" :show="item.isLoading" :color="themeColor" textColor="#ccc" text="加载中"
                        :duration="1500">
                    </u-loading-icon>

                    <!-- 展开更多二级评论 -->
                    <view class="reply-more" @tap="showAllReply(index, item)"
                        v-if="item.allReply && Number(item.allReply) - item.replyList.length">
                        <view class="left-line" />
                        展开{{ item.allReply - item.replyList.length }}条回复
                        <u-icon class="more" name="arrow-down" :size="20"></u-icon>
                    </view>

                    <!-- 收起二级评论 -->
                    <view class="reply-more flex-row-center" @tap="closetoAllReply(index)"
                        v-if="item.allReply && Number(item.allReply) === item.replyList.length">
                        <view class="left-line" />
                        收起回复
                        <u-icon class="more" name="arrow-up" :size="20"></u-icon>
                    </view>


                </view>
            </view>
        </view>


        <!-- 评论弹框 -->
        <uni-popup ref="revertRef" type="bottom" class="comment-pop" @change="onChangePop">
            <view class="comment-pop-content">
                <up-input :placeholder="placeholder" border="surround" shape="circle" v-model="inputVal"
                    :focus="comtIptFocusStatus" @change="onChangeIput"></up-input>
                <view class="send" :style="{ background: themeColor }" @click="sendComment">发送</view>
            </view>
        </uni-popup>
    </view>
</template>

<script setup>
import { ref } from 'vue'
import { onLoad } from '@dcloudio/uni-app'


/**
 * 评论组件
 * @props list 评论列表
 * @props modelValue 评论 输入框的value ==> 可使用 v-model 语法糖
 * @props themeColor 主题色
 * @props keyNames 指定keyName 详见文档
 * @event getLike 点赞事件
 * @event sendComment 发送评论事件
 * @event getMore 获取更多评论事件
 * @event remove 删除评论事件
 * 
 * @export close 发送评论成功可调用 【close方法】 关闭评论弹框
 *
 * @example <h-comment-box :list="评论列表" v-model="输入框的值" @getLike="点赞回调" @sendComment="发送评论回调" ref="hCommentRef"/>
 */

/**
 * 定义props
 * @param list 评论列表
 * @param modelValue 评论 输入框的value
 * @param themeColor 主题色
 * @param keyNames 指定keyName
 */
const props = defineProps({
    list: {
        type: Array,
        default: () => []
    },
    modelValue: {
        type: String,
        default: ''
    },
    themeColor: {
        type: String,
        default: '#1DBFB6'
    },
    keyNames: {
        type: Object,
        default: () => {
            return {
                // 一级评论相关
                id: 'id', // 评论id
                user_id: 'user_id', // 用户id
                user_name: 'user_name', // 用户名
                user_avatar: 'user_avatar', // 用户头像
                user_content: 'user_content', // 用户评论内容
                user_date: 'user_date', // 用户评论时间
                user_is_like: 'user_is_like', // 用户是否点赞
                user_like_num: 'user_like_num', // 用户点赞数
                isLoading: 'isLoading', // 是否显示加载中
                allReply: 'allReply', // 评论总数
                isMyComment:'isMyComment', // 是否是自己的评论
                // 二级评论相关
                user_reply_list: 'user_reply_list',  // 回复列表
                user_reply_id: 'user_reply_id', // 回复人id
                user_reply_name: 'user_reply_name', // 回复人名字
                user_reply_avatar: 'user_reply_avatar', // 回复人头像
                user_reply_content: 'user_reply_content', // 回复 内容
                user_reply_date: 'user_reply_date', // 回复 时间
                user_reply_is_like: 'user_reply_is_like', // 回复人是否点赞
                user_reply_like_num: 'user_reply_like_num', // 回复人点赞数
                pid: 'pid',
                user_isMyComment:'user_isMyComment', // 是否是自己的评论
                to_user_name: 'to_user_name', // 被回复人名字
                to_user_id: 'to_user_id', // 被回复人id
            }
        }
    }

})

// 定义主题色
const themeColor = ref('#1DBFB6')

// 输入框的placeholde
const placeholder = ref('请输入内容')

// 输入框的value
const inputVal = ref('')

const revertRef = ref(null) // uni_popup 弹框实例
const comtIptFocusStatus = ref(false) // 评论输入框聚焦状态


const isComtIndex = ref(0) // 回复：当前点击的一级评论索引
const isComtRow = ref({}) // 回复：点击的 一级评论信息
const isComtIndex_s = ref(0) // 回复：点击的 二级评论索引
const isComtRow_s = ref({}) // 回复：点击的 二级评论信息



/**
 * 弹框change事件
 */
function onChangePop(e) {
    comtIptFocusStatus.value = e.show
}



/**
 * 发送评论
 */
function sendComment() {
    /**
     * @argument index 一级评论索引
     * @argument row 一级评论信息
     * @argument index_s 二级评论索引
     * @argument row_s 二级评论信息
     * @callback ()=>{} 发送评论接口成功之后 调用sendComment的第二个形参 该形参为方法，更新组件内的评论列表
     */
    emit('sendComment', {
        index: isComtIndex.value,
        row: isComtRow.value,
        index_s: isComtIndex_s.value,
        row_s: isComtRow_s.value,
    }, (data) => {
        if (isComtIndex_s.value !== null) {
            commentList.value[isComtIndex.value].replyList.unshift({...data,isMyComment:true})

        } else {
            commentList.value.unshift({...data,isMyComment:true})
        }
        close()
    })
}




/**
 * 定义事件
 */
const emit = defineEmits(['getLike', 'sendComment', 'getMore', 'remove', 'update:modelValue'])
defineExpose({
    close
})



// 评论列表 ==》 根据传入的commentList和传入的keyNames生成新的评论列表
const commentList = ref([])


onLoad(() => {
    commentList.value = props.list.map((item) => {
        return {
            id: item[props.keyNames.id],
            name: item[props.keyNames.user_name],
            url: item[props.keyNames.user_avatar],
            contentText: item[props.keyNames.user_content],
            date: item[props.keyNames.user_date],
            isLike: item[props.keyNames.user_is_like],
            likeNum: item[props.keyNames.user_like_num],
            isLoading: item[props.keyNames.isLoading],
            allReply: item[props.keyNames.allReply],
            isMyComment:item[props.keyNames.isMyComment],
            replyList: item[props.keyNames.user_reply_list].map((item_s) => {
                return {
                    id: item_s[props.keyNames.user_reply_id],
                    pid: item_s[props.keyNames.pid],
                    name: item_s[props.keyNames.user_reply_name],
                    url: item_s[props.keyNames.user_reply_avatar],
                    contentStr: item_s[props.keyNames.user_reply_content],
                    date: item_s[props.keyNames.user_reply_date],
                    isLike: item_s[props.keyNames.user_reply_is_like],
                    likeNum: item_s[props.keyNames.user_reply_like_num],
                    to_user_name: item_s[props.keyNames.to_user_name],
                    to_user_id: item_s[props.keyNames.to_user_id],
                    user_isMyComment:item_s[props.keyNames.user_isMyComment],
                }
            })
        }
    })
    console.log(commentList.value, 'list');
})







/**
* 回复评论弹框
 * @param index  一级评论索引
 * @param row   一级评论信息
 * @param j  二级评论索引
 * @param row_s 二级评论信息
*/
function showRevertPop(index, row, j, row_s) {
    isComtIndex.value = index // 保存当前评论,发送评论时使用
    isComtRow.value = { // 保存当前评论,发送评论时使用
        id: row.id,
        name: row.name,
        url: row.url,
        contentText: row.contentText,
        date: row.date,
        isLike: row.isLike,
        likeNum: row.likeNum,
        allReply: row.allReply,
        isMyComment:row.isMyComment,
    }
    if (j !== null) isComtIndex_s.value = j // 保存当前评论,发送评论时使用
    if (row_s !== null) {
        isComtRow_s.value = {
            id: row_s.id,
            pid: row_s.pid,
            name: row_s.name,
            url: row_s.url,
            contentStr: row_s.contentStr,
            date: row_s.date,
            isLike: row_s.isLike,
            likeNum: row_s.likeNum,
            user_isMyComment:row_s.user_isMyComment,
        }
    }

    placeholder.value = `回复 @${j !== null ? row_s.name : row.name}`
    openPop()
}




/**
 * 删除评论
 * @param index  一级评论索引
 * @param row   一级评论信息
 * @param j  二级评论索引
 * @param row_s 二级评论信息
 */
function removeRevertModal(index, row, j, row_s) {
    uni.showModal({
        title: '提示',
        content: j === null? '确定删除该评论吗？' : '确定删除该回复吗？',
        success: (res) => {
            if (res.confirm) {
                emit('remove', {
                    index,
                    row,
                    j,
                    row_s
                }, () => {
                    if (j !== null) {
                        commentList.value[index].replyList.splice(j, 1)
                        commentList.value[index].allReply--
                    } else {
                        commentList.value.splice(index, 1)
                    }
                })
            }
        }
    })

}




/**
 * 打开评论弹框
 */
function openPop() {
    revertRef.value.open()
}




/**
 * 关闭评论弹框
 * @param msg 提示信息 不传则不提示
 */
function close(msg) {
    revertRef.value.close()
    inputVal.value = ''
    if (msg) uni.showToast({ title: msg, icon: 'none' })
}




/**
 * 收起回复
 */
function closetoAllReply(index) {
    commentList.value[index].replyList = []
}




/**
 * 展开更多评论
 * @param index 一级评论索引
 */
function showAllReply(index, row) {
    commentList.value[index].isLoading = true // 显示加载中
    emit('getMore', { index, row }, (data) => {
        let newData = null

        if (Array.isArray(data) && data.length) {
            newData = data.map((item) => {
                return {
                    id: item[props.keyNames.user_reply_id],
                    pid: item[props.keyNames.pid],
                    name: item[props.keyNames.user_reply_name],
                    url: item[props.keyNames.user_reply_avatar],
                    contentStr: item[props.keyNames.user_reply_content],
                    date: item[props.keyNames.user_reply_date],
                    isLike: item[props.keyNames.user_reply_is_like],
                    likeNum: item[props.keyNames.user_reply_like_num],
                    to_user_id: item[props.keyNames.to_user_id],
                    to_user_name: item[props.keyNames.to_user_name],
                    user_isMyComment:item[props.keyNames.user_isMyComment],
                }
            })

            commentList.value[index].replyList.push(...newData)

        } else {
            newData = {
                id: item[props.keyNames.user_reply_id],
                pid: item[props.keyNames.pid],
                name: item[props.keyNames.user_reply_name],
                url: item[props.keyNames.user_reply_avatar],
                contentStr: item[props.keyNames.user_reply_content],
                date: item[props.keyNames.user_reply_date],
                isLike: item[props.keyNames.user_reply_is_like],
                likeNum: item[props.keyNames.user_reply_like_num],
                to_user_id: item[props.keyNames.to_user_id],
                to_user_name: item[props.keyNames.to_user_name],
                user_isMyComment:item[props.keyNames.user_isMyComment],
            }

            commentList.value[index].replyList.push(newData)

        }

        commentList.value[index].isLoading = false // 关闭加载中

    })
}




/**
 * 点赞
 * @param index 一级评论索引
 * @param j 二级评论索引
 */
function getLike(index, j) {
    /**
     * index 一级评论索引
     * j 二级评论索引
     * ()=>{} 点在接口成功之后 调用getLike的第二个形参 该形参为方法，更新组件内的点赞样式
     */
    emit('getLike', { index, j }, () => {
        if (j !== undefined) {
            commentList.value[index].replyList[j].isLike = !commentList.value[index].replyList[j].isLike
            if (commentList.value[index].replyList[j].isLike == true) {
                commentList.value[index].replyList[j].likeNum++;
            } else {
                commentList.value[index].replyList[j].likeNum--;
            }

        } else {
            commentList.value[index].isLike = !commentList.value[index].isLike;
            if (commentList.value[index].isLike == true) {
                commentList.value[index].likeNum++;
            } else {
                commentList.value[index].likeNum--;
            }

        }
    })
}



/**
 * 输入框内容改变 => 更新v-model
 */
function onChangeIput(e) {
    emit('update:modelValue', e)
}








</script>

<style lang="scss" scoped>
.h_comment {
    display: flex;
    padding: 30rpx 30rpx 0 30rpx;
    background-color: #fff;

    .h_left {
        image {
            width: 64rpx;
            height: 64rpx;
            border-radius: 50%;
            background-color: #f2f2f2;
        }
    }

    .h_right {
        flex: 1;
        padding-left: 20rpx;
        padding-top: 6rpx;
        font-size: 30rpx;


        .h_info {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 10rpx;



            .like {
                display: flex;
                align-items: center;
                font-size: 26rpx;

                .num {
                    margin-right: 4rpx;
                }
            }


        }

        .h_content {
            margin-bottom: 10rpx;
        }


        .h_date {
            display: flex;
            font-size: 24rpx;
            color: #9a9a9a;


            .h_revert {
                margin-left: 10rpx;
            }

            .h_remove {
                color: #868686;
                margin-left: 20rpx;

            }
        }



        .h_revert_list {
            background-color: rgb(251, 251, 251);
            border-radius: 12rpx;
            margin-top: 20rpx;
            padding: 0 20rpx;

            .item_s {
                padding-top: 20rpx;


                .user-info {
                    display: flex;
                    align-items: center;


                    .username {
                        flex: 1;
                        display: flex;
                        align-items: center;
                        font-size: 24rpx;
                        color: #999999;
                        margin-left: 10rpx;

                        .left-name {
                            margin-right: 10rpx;
                        }

                        .right-name {
                            margin-left: 10rpx;
                        }

                    }


                    .like {
                        display: flex;
                        align-items: center;
                        color: #9a9a9a;
                        font-size: 26rpx;

                        .num {
                            margin-right: 4rpx;
                            color: #9a9a9a;
                        }
                    }


                }

                .text {
                    padding: 8rpx 0;
                }

                .date {
                    display: flex;
                    font-size: 24rpx;
                    color: #9a9a9a;


                    .revert {
                        margin-left: 10rpx;
                    }

                    .h_remove {
                        color: #868686;
                        margin-left: 20rpx;
                    }
                }
            }


            .reply-more {
                width: 100%;
                padding-top: 20rpx;
                display: flex;
                color: #868686;
                align-items: center;
                font-size: 24rpx;
                padding-bottom: 20rpx;


                .left-line {
                    width: 50rpx;
                    height: 2rpx;
                    background-color: #eee;
                    margin-right: 10rpx;
                }


                .more {
                    margin-left: 6rpx;
                }
            }



        }
    }
}




::v-deep .vue-ref {
    padding-bottom: 0 !important;
}




.comment-pop-content {
    height: 100rpx;
    width: 750rpx;
    background-color: #fff;
    display: flex;
    align-items: center;

    .send {
        display: flex;
        align-items: center;
        justify-content: center;
        width: 120rpx;
        height: 40rpx;
        border-radius: 60rpx;
        font-size: 28rpx;
        font-weight: 500;
        color: #ffffff;
        margin-left: 18rpx;
    }
}

.p-t-20 {
    padding-top: 20rpx;
}


.flex-row-center {
    justify-content: center;
}
</style>
