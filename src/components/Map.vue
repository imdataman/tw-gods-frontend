<template>
  <div class="map">
    <div class="mapWrapper">
      <div class="mapContainer" v-for="god in godsList" :key="god.id">
        <div class="mapText">
          <h3>{{ god.name }}</h3>
          <ol>
            <li
              v-for="(l, i) in god.highlight"
              :key="`${god.name}${i}`"
              v-html="`<b>${l.location} - ${l.temple}</b></br>${l.description}`"
            ></li>
          </ol>
        </div>
        <svg :viewBox="`0, 0, ${width}, ${height}`">
          <g class="background">
            <path :d="path(background)"></path>
          </g>
          <g class="grid">
            <path
              v-for="block in grid"
              :key="`${god.id}${block.properties.id}`"
              :d="path(block)"
              :style="{
                fill: colorScale.get(god.id)(block.properties[god.id])
              }"
            ></path>
          </g>
          <g class="outline">
            <path :d="path(outline)"></path>
          </g>
          <g
            class="highlight"
            v-for="location in god.highlight"
            :key="location.location"
          >
            <text
              :x="projection(location.coord)[0]"
              :y="projection(location.coord)[1]"
              dy="-6"
            >
              {{ location.id }}
            </text>
            <circle
              :cx="projection(location.coord)[0]"
              :cy="projection(location.coord)[1]"
              r="1.5"
            ></circle>
            <!-- <polyline :points="polyline.get(god.id)"></polyline> -->
          </g>
        </svg>
      </div>
    </div>
  </div>
</template>

<script>
import grid from "@/assets/data/final-grid.json";
import county from "@/assets/data/tw-county.json";

import { extent } from "d3-array";
import { geoPath, geoConicEqualArea } from "d3-geo";
import { scaleQuantize } from "d3-scale";

import * as topojson from "topojson-client";

export default {
  name: "Map",
  data() {
    return {
      grid: topojson.feature(grid, grid.objects["final-grid"]).features,
      county: county,
      godsList: [
        {
          name: "福德正神",
          id: "fude",
          re: "福德正神|土地公",
          highlight: [
            {
              id: 1,
              location: "屏東恆春",
              temple: "高山巖福德宮",
              description: "台灣最老土地公廟",
              coord: [120.719604, 21.9633437]
            },
            {
              id: 2,
              location: "基隆",
              temple: "福德宮",
              description: "地方信仰中心",
              coord: [121.7419687, 25.1280497]
            },
            {
              id: 3,
              location: "宜蘭四結",
              temple: "福德廟",
              description: "台灣最大的金身土地公",
              coord: [121.7656373, 24.6973611]
            },
            {
              id: 4,
              location: "台中",
              temple: "惠來里福德祠",
              description: "台中市地價最值錢的土地公廟",
              coord: [120.6442008, 24.1605017]
            },
            {
              id: 5,
              location: "桃園",
              temple: "土地公文化館",
              description: "桃園區土地公廟密度全國第一",
              coord: [121.321273, 24.9981896]
            }
          ]
        },
        {
          name: "觀世音菩薩",
          id: "guanyin",
          re: "觀世音|觀音",
          highlight: [
            {
              id: 1,
              location: "澎湖馬公",
              temple: "觀音亭",
              description: "全台唯一由清代澎湖廳總鎮衙署石獅鎮守的寺廟",
              coord: [119.5620759, 23.5676239]
            },
            {
              id: 2,
              location: "金門",
              temple: "靈濟寺",
              description: "相傳創建於唐朝，已有八百年歷史",
              coord: [118.3185126, 24.4316216]
            },
            {
              id: 3,
              location: "新竹北埔",
              temple: "慈天宮",
              description: "地方信仰中心",
              coord: [121.0582618, 24.6997603]
            }
          ]
        },
        {
          name: "媽祖",
          id: "mazu",
          re: "天上聖母|媽祖",
          highlight: [
            {
              id: 1,
              location: "彰化芳苑",
              temple: "普天宮",
              description: "全台灣面積最大的媽祖廟",
              coord: [120.3168645, 23.9290597]
            },
            {
              id: 2,
              location: "高雄旗山",
              temple: "天后宮",
              description: "地方信仰中心",
              coord: [120.4823064, 22.8866163]
            },
            {
              id: 3,
              location: "台東",
              temple: "天后宮",
              description: "以炮炸寒單爺聞名",
              coord: [121.1554925, 22.7567605]
            },
            {
              id: 4,
              location: "新北貢寮",
              temple: "仁和宮",
              description: "地方信仰中心",
              coord: [121.9229976, 25.0517402]
            }
          ]
        },
        {
          name: "釋迦牟尼",
          id: "buddha",
          re: "釋迦牟尼|釋迦佛",
          highlight: [
            {
              id: 1,
              location: "南投埔里",
              temple: "中台禪寺",
              description: "世界最大、最高的佛教寺廟",
              coord: [120.9445887, 24.0090905]
            },
            {
              id: 2,
              location: "花蓮",
              temple: "慈濟文化園區",
              description: "慈濟全球事業總樞紐",
              coord: [121.5907229, 23.9938778]
            },
            {
              id: 3,
              location: "新北金山",
              temple: "法鼓山",
              description: "台灣四大名山之一",
              coord: [121.614875, 25.239825]
            }
          ]
        },
        {
          name: "王爺／千歲",
          id: "wangye",
          re: "千歲|王爺",
          highlight: [
            {
              id: 1,
              location: "台南北門",
              temple: "南鯤鯓代天府",
              description: "南瀛古三大代天府",
              coord: [120.1430727, 23.2863908]
            },
            {
              id: 2,
              location: "澎湖",
              temple: "澎湖縣",
              description: "全澎王爺廟間數高居全澎廟宇之冠",
              coord: [119.621856, 23.564766]
            },
            {
              id: 3,
              location: "金門",
              temple: "金門縣",
              description: "金門所供奉的王爺有四十多姓",
              coord: [118.3799582, 24.447835]
            },
            {
              id: 4,
              location: "屏東東港",
              temple: "東隆宮",
              description: "以東港燒王船聞名",
              coord: [120.4488088, 22.4630697]
            }
          ]
        },
        {
          name: "玄天上帝",
          id: "xuanwu",
          re: "玄天|真武|北極大帝",
          highlight: [
            {
              id: 1,
              location: "南投名間",
              temple: "松柏嶺受天宮",
              description: "祀奉玄天上帝的總廟",
              coord: [120.6302176, 23.8348126]
            },
            {
              id: 2,
              location: "雲林斗六",
              temple: "斗六市",
              description: "斗六玄天上帝廟宇間數全縣最多",
              coord: [120.5409148, 23.7078134]
            }
          ]
        }
      ],
      width: 300,
      height: 410,
      colorPalette: ["#eff3ff", "#bdd7e7", "#6baed6", "#2171b5"],
      highlightLength: 40
    };
  },
  computed: {
    projection() {
      return geoConicEqualArea()
        .rotate([-124.32, 0.0])
        .center([0.0, 23.65])
        .parallels([10, 30.0])
        .scale(5000)
        .precision(0.1);
    },
    path() {
      return geoPath().projection(this.projection);
    },
    colorScale() {
      return new Map(
        this.godsList.map(d => [
          d.id,
          scaleQuantize()
            .domain(extent(this.grid.map(j => j.properties[d.id])))
            .range(this.colorPalette)
        ])
      );
    },
    polyline() {
      return new Map(
        this.godsList.map(d => {
          const x = this.projection(d.highlight[0].coord)[0];
          const y = this.projection(d.highlight[0].coord)[1];
          const l = this.highlightLength;
          return [d.id, `${x},${y} ${x - l},${y}`];
        })
      );
    },
    background() {
      return topojson.feature(county, county.objects["tw-county"]);
    },
    outline() {
      return topojson.mesh(county);
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.mapWrapper {
  padding: 20px 0;
  display: flex;
  justify-content: space-evenly;
  flex-wrap: wrap;
}
.mapContainer {
  width: 450px;
  position: relative;
  padding-bottom: 50px;
}

.mapText {
  position: absolute;
  left: 1rem;
  top: 1rem;
  color: black;
  ol {
    padding-inline-start: 1rem;
  }
}

.background {
  fill: lightgray;
}

.outline {
  fill: none;
  stroke: black;
  stroke-width: 0.5px;
  stroke-dasharray: 1;
}

.grid {
  stroke: none;
}

.highlight {
  polyline {
    stroke: black;
    stroke-width: 0.75px;
  }
  text {
    font-size: 0.75rem;
    font-weight: 600;
    text-anchor: middle;
    dominant-baseline: middle;
    fill: white;
    paint-order: stroke;
    stroke: black;
    stroke-width: 0.75px;
  }
  circle {
    fill: white;
    stroke: black;
    paint-order: stroke;
    stroke-width: 0.75px;
  }
}

@media (max-width: 480px) {
  .mapContainer {
    width: 90vw;
  }
  .mapText {
    font-size: 0.8rem;
    li {
      font-size: 0.5rem;
    }
  }
}
</style>
