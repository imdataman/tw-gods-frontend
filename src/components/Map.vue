<template>
  <div class="map">
    <div class="mapWrapper">
      <div class="mapContainer" v-for="god in godsList" :key="god.id">
        <div class="mapTitle">
          <h3>{{ god.name }}</h3>
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
        { name: "福德正神", id: "fude", re: "福德正神|土地公" },
        { name: "觀世音菩薩", id: "guanyin", re: "觀世音|觀音" },
        { name: "媽祖", id: "mazu", re: "天上聖母|媽祖" },
        { name: "釋迦牟尼", id: "buddha", re: "釋迦牟尼|釋迦佛" },
        { name: "王爺／千歲", id: "wangye", re: "千歲|王爺" },
        { name: "玄天上帝", id: "xuanwu", re: "玄天|真武|北極大帝" }
      ],
      width: 300,
      height: 400,
      colorPalette: ["#eff3ff", "#bdd7e7", "#6baed6", "#3182bd", "#08519c"]
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
  margin: auto;
  max-width: 1000px;
  display: flex;
  justify-content: space-evenly;
  flex-wrap: wrap;
}
.mapContainer {
  width: 450px;
  position: relative;
  padding-bottom: 50px;
}

.mapTitle {
  position: absolute;
  left: 1rem;
  top: 1rem;
  color: black;
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

@media (max-width: 480px) {
  .mapContainer {
    width: 90vw;
  }
  .mapTitle {
    font-size: 0.8rem;
  }
}
</style>
