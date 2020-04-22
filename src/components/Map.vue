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
import godsList from "@/assets/data/gods-list.json";

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
      godsList: godsList,
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
