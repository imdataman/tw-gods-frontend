<template>
  <div id="app">
    <div class="opening">
      <h2>台灣廟宇地圖</h2>
      <p>
        本文使用內政部的「108年度全國宗教資訊寺廟分布圖」，搭配熱點分析（Hotspot
        Analysis）技術，分析台灣各主要信仰的地理分佈。地圖的顏色愈深，代表主祀該神祇的廟宇在該地區的廟宇佔比愈高。本文分析六大信仰：福德正神（土地公）、觀世音菩薩、媽祖（天上聖母）、釋迦牟尼、王爺／千歲及玄天上帝。
      </p>
    </div>
    <Map />
    <div class="footnote">
      <span>
        資料來源：<a
          target="_blank"
          href="https://segis.moi.gov.tw/STAT/Generic/Project/GEN_STAT.ashx?method=downloadproductfile&code=433B6C121D32439A6239FE8CBE94A3D3&STTIME=108Y&STUNIT=null&BOUNDARY=全國"
          >內政部</a
        >
      </span>
      <span class="sourceCode"
        >製作：<a href="https://www.facebook.com/imDataMan/" target="_blank"
          >林佳賢</a
        ><a href="https://github.com/imdataman/tw-county-square" target="_blank"
          ><img src="@/assets/GitHub-Mark-32px.png"
        /></a>
      </span>
    </div>
  </div>
</template>

<script>
import Map from "./components/Map.vue";

export default {
  name: "App",
  components: {
    Map
  }
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  max-width: 1000px;
  margin: auto;
}

a {
  text-decoration: none;
  color: darkblue;
  &:hover {
    color: orange;
  }
}

.opening,
.footnote {
  padding-left: 10%;
  padding-right: 10%;
  max-width: 1000px;
  p {
    word-wrap: break-word;
  }
  span {
    font-size: 0.75rem;
    img {
      height: 0.75rem;
      padding-left: 0.25rem;
      vertical-align: middle;
    }
  }
}

.footnote {
  padding-bottom: 20px;
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
}

.sourceCode {
  text-align: center;
}
</style>
