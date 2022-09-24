<script setup>
import { computed, onMounted, ref } from "vue";
const progress = ref(0);

onMounted(() => {
  const app = document.querySelector("#app");
  const section = document.querySelector("#section2");
  function scrollHandler() {
    if (app.scrollTop < window.innerHeight) {
      progress.value = 0;
      // 算直向區域有幾個
    } else if (app.scrollTop > window.innerHeight * 3) {
      progress.value = 1;
    } else {
      // 捲軸滾動要幾個區域 2就是要捲２個區域
      const percent =
        (app.scrollTop - window.innerHeight) / (window.innerHeight * 2);
      progress.value = percent;
    }
    // -------
    // *垂直捲軸百分比 控制  水平捲軸距離
    // -------
    // 捲軸移動距離 = 捲軸寬度 - 視窗寬度 = section.scrollWidth - window.innerWidth
    // 水平捲軸實際距離 = 捲軸移動距離 * 垂直捲軸百分比
    let position = (section.scrollWidth - window.innerWidth) * progress.value;
    section.scrollTo({
      left: position,
    });
  }
  app.addEventListener("scroll", scrollHandler);
});

const fixClass = computed(() => {
  return progress.value < 1;
});
</script>

<template>
  <div id="section1" class="section text">俐落薄型新詮釋。</div>
  <div id="section2" class="section" :class="{ sticky: fixClass }">
    <div class="section text bg-R">1</div>
    <div class="section text bg-G">2</div>
    <div class="section text bg-B">3</div>
  </div>
  <div id="section3" class="section text bg-G" :class="{ static: fixClass }">
    4
  </div>
  <div id="section4" class="section text bg-B" :class="{ static: fixClass }">
    5
  </div>
  <div id="section5" class="section text" :class="{ static: fixClass }">
    相機與音響，再進化。
  </div>
</template>

<style lang="scss">
* {
  margin: 0;
  padding: 0;
  outline: 1px solid red;
}
body {
  overflow: hidden; // 隱藏 瀏覽器滾動條
}
.text {
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 60px;
  font-weight: 700;
}
#app {
  height: 100vh;
  overflow: auto; // 讓sticky 有作用
}
.section {
  height: 100%;
  position: relative;
}
#section2 {
  display: flex;
  flex-wrap: nowrap;
  overflow: hidden; // 隱藏超出的元素或圖片
}
#section2 > .section {
  width: 100vw;
  min-width: 100%;
}
#section3 {
  background-color: pink;
}
#section4 {
  background-color: darkgoldenrod;
}
.sticky {
  position: sticky !important;
  top: 0;
  left: 0;
}
.static {
  position: static !important;
}
.absolute {
  position: absolute !important;
}
.relative {
  position: relative !important;
}
.bg-R {
  background-color: red;
}
.bg-G {
  background-color: green;
}
.bg-B {
  background-color: blue;
}
</style>
