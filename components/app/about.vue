<script setup lang="ts">
const aboutRef = ref();
useSectionInView(aboutRef, "关于我");
</script>

<template>
  <section
    ref="aboutRef"
    class="max-w-[45rem] text-start leading-8 sm:mb-40 scroll-mt-28 mb-28"
    id="about"
    v-motion
    :initial="{
      opacity: 0,
      y: 100,
    }"
    :enter="{
      opacity: 1,
      y: 0,
    }"
    :delay="175"
  >
    <h2 class="text-3xl font-medium capitalize mb-8 text-center">关于我</h2>

    <p>
      18年本科毕业后一直在从事相关的开发工作，有丰富的项目实战经验，熟悉开发流程。在编程的挑战中，我乐在其中，与团队合作开发项目、解决复杂问题的过程带来巨大的满足感。我熟练使用
      SpringBoot，SpringCould，Mybatis 和 Vue3 等技术，对 JavaScript, TypeScript 和 HTML/CSS
      也有很深的理解 💻。平时，我喜欢学习新技能并做一些有趣的项目。
      如果不在电脑前，我喜欢做饭、看电影，规律的睡眠和饮食是我精力充沛的秘诀 🌟。
    </p>
  </section>
</template>
