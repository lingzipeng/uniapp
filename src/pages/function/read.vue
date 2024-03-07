<template>
  <view class="container">
    <uni-icons class="next-article" @click="getNextArticle" type="right" size="30"></uni-icons>
    <view class="title">{{ article.title }}</view>
    <view class="content">{{ article.content}}</view>
  </view>
</template>

<script setup lang="ts">
import { ref, computed } from "vue";

const articles = ref([
  {
    title: "狼来了！",
    content: "从前，在一个小村庄里，住着一个善良而勤劳的农夫，他名叫杰克。杰克有一群可爱的动物，包括一群羊、几头牛和一只聪明的狗，它们共同生活在一片宁静的农场里。有一天，村庄里传来了一个消息：狼出现了！人们都被吓坏了，纷纷关起了家门，怕狼闯进来伤害他们的家人和家畜。杰克也听说了这个消息，于是决定采取措施来保护自己的农场。",
  },
  {
    title: "文章标题2",
    content: "文章内容2",
  },
  {
    title: "文章标题3",
    content: "文章内容3",
  },
  // 其他文章
]);
const currentIndex = ref<number>(0);

const article = computed(() => articles.value[currentIndex.value]);

const getNextArticle = () => {
  currentIndex.value = (currentIndex.value + 1) % articles.value.length;
};
</script>

<style scoped>
.container {
  padding: 20px;
}

.title {
  font-size: 20px;
  font-weight: bold;
  margin-bottom: 10px;
}

.content {
  line-height: 1.6;
  margin-bottom: 20px;
}

.next-article {
  background-color: #ceeaf0;
  color: #fff;
  position: absolute;
  border-radius: 4px;
  width: 60px;
  height: 30px;
  left: 220px;
  cursor: pointer;
  text-align: center;
}
</style>
