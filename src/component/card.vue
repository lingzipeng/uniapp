<template>
    <uni-section title="卡片+列表" type="line">
    <uni-card padding="0" spacing="0">
        <template v-slot:cover>
            <view class="custom-cover">
                <image class="cover-image" mode="aspectFill" :src="cover">
                </image>
                <view class="cover-content">
                    <text class="uni-subtitle uni-white">今日新闻热点</text>
                </view>
            </view>
        </template>
        <uni-list>
            <uni-list-item title="今日新闻" showArrow></uni-list-item>
            <uni-list-item title="今日新闻" showArrow></uni-list-item>
        </uni-list>
        <view slot="actions" class="card-actions no-border">
            <view class="card-actions-item" @click="actionsClick('分享')">
                <uni-icons type="heart" size="18" color="#999"></uni-icons>
                <text class="card-actions-item-text">分享</text>
            </view>
            <view class="card-actions-item" @click="actionsClick('点赞')">
                <uni-icons type="heart" size="18" color="#999"></uni-icons>
                <text class="card-actions-item-text">点赞</text>
            </view>
            <view class="card-actions-item" @click="actionsClick('评论')">
                <uni-icons type="chatbubble" size="18" color="#999"></uni-icons>
                <text class="card-actions-item-text">评论</text>
            </view>
        </view>
    </uni-card>
</uni-section>
</template>
<script setup lang="ts">
const cover = 'https://qiniu-web-assets.dcloud.net.cn/unidoc/zh/shuijiao.jpg';

const onClick = (e: MouseEvent) => {
    console.log(e);
};

const actionsClick = (text: string) => {
    uni.showToast({
        title: text,
        icon: 'none'
    });
};
</script>
