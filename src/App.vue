<template>
  <div id="app">
    <div class="opening">
      <h2>台灣眾神：各家信仰與祂們的管區</h2>
      <p>
        可能很少有其他國家像台灣一樣，每個家族信仰的神明都不大一樣。根據108年全國寺廟統計，有登記的寺廟就有將近1.1萬間，種類更有至少200種（參考<a
          href="http://crgis.rchss.sinica.edu.tw/crgis_tw/crgis/refence_gods.html"
          target="_blank"
          >中研院神祇名稱對照表</a
        >）。
      </p>
      <p>
        這麼多的神明擠在這座小小的島上，有沒有自己的地盤？根據有登記立案的主祀廟宇間數，我們可以找出台灣的十大佛、道及民間信仰：
      </p>
      <p>
        王爺千歲（1446間）、土地公（1230間）、釋迦牟尼（1020間）、觀音菩薩（1003間）、媽祖（967間）、玄天上帝（732間）、關聖帝君（527間）、保生大帝（289間）、中壇元帥（261間）、陰神（250間）。
      </p>
      <p>
        這些信仰如何在台灣劃分各自的管區？本文分析廟宇的分佈情況，找出前九大信仰的熱區，並列出熱區的相關廟宇或地點供參考。
      </p>
    </div>
    <Map />
    <div class="footnote">
      <p class="methodology">
        研究方法：本文使用內政部的「108年度全國宗教資訊寺廟分布圖」，搭配熱點分析（Hotspot
        Analysis）的Getis-Ord
        Gi演算法，分析台灣各主要信仰的地理分佈。地圖的每個方格長寬為5公里，無廟宇的地區為灰色。先計算各個神明主祀廟宇佔方格中所有廟宇的比率，再運用演算法計算出信仰熱度，紫色的顏色愈深，代表主祀該神祇的廟宇在該地區的廟宇佔比傾向愈高。本文使用中央研究院文化資源地理資訊系統的神祇名稱分類，找出各個神祇的主祀廟宇。
      </p>
      <div class="credit">
        <span>
          資料來源：<a
            target="_blank"
            href="https://data.moi.gov.tw/MoiOD/Data/DataDetail.aspx?oid=1B56C087-43D9-4B4B-B08C-D7B6A750E033"
            >內政部</a
          >
        </span>
        <span class="sourceCode"
          >製作：<a href="https://www.facebook.com/imDataMan/" target="_blank"
            >林佳賢</a
          ><a
            href="https://github.com/imdataman/tw-gods-frontend"
            target="_blank"
            ><img src="@/assets/GitHub-Mark-32px.png"
          /></a>
        </span>
      </div>
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

<style scoped lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  max-width: 1000px;
  margin: auto;
}

a {
  color: black;
  &:hover {
    color: goldenrod;
  }
}

h2 {
  color: goldenrod;
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
      height: 1rem;
      padding-left: 0.25rem;
      vertical-align: sub;
    }
  }
}

.methodology {
  font-size: 0.75rem;
  opacity: 0.7;
}

.credit {
  padding-bottom: 20px;
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  opacity: 0.7;
}

.sourceCode {
  text-align: center;
}
</style>
