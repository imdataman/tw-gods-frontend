<template>
  <div id="app">
    <div class="opening">
      <h2>台灣眾神：六大神明與祂們的地盤</h2>
      <p>
        可能很少有其他國家像台灣一樣，每個家族信仰的神明都不大一樣。根據108年全國寺廟統計，有登記的寺廟就有將近1.1萬間，其中若不考慮別名和多重主祀神祇的狀況，1.1萬間的寺廟，主祀神祇竟多達1700種。
      </p>
      <p>
        這麼多的神明擠在這座小小的島上，有沒有自己的地盤？根據主祀廟宇的間數，我們可以找出台灣的六大民間信仰：<br />王爺千歲（1386間）、土地公（1167間）、釋迦牟尼（966間）、觀音（949間）、媽祖（920間）和玄天上帝（681間）。
      </p>
      <p>
        這六個重量級的神明怎麼劃定各自的勢力範圍？本文分析廟宇的分佈情況，找出六大信仰的熱區，並列出熱區的相關廟宇供參考。
      </p>
    </div>
    <Map />
    <div class="footnote">
      <p class="methodology">
        研究方法：本文使用內政部的「108年度全國宗教資訊寺廟分布圖」，搭配熱點分析（Hotspot
        Analysis）的Getis-Ord
        Gi演算法，分析台灣各主要信仰的地理分佈。地圖的每個方格長寬為5公里，無廟宇的地區為灰色。這些方格會根據演算法計算出的信仰熱度分成四大群，藍色的顏色愈深，代表主祀該神祇的廟宇在該地區的廟宇佔比傾向愈高。本文分析的六大信仰如下：福德正神（土地公）、觀世音菩薩（觀音）、媽祖（天上聖母）、釋迦牟尼、王爺／千歲及玄天上帝（真武大帝、北極大地），只要原資料的主祀神祇欄位有提到上述名稱，即算入該信仰廟宇。
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
  text-decoration: none;
  color: midnightblue;
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
}

.credit {
  padding-bottom: 20px;
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
}

.sourceCode {
  text-align: center;
}
</style>
