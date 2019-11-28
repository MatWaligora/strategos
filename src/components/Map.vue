<template>
  <div class="map">
    <div v-for="(x, indexX) in gameMap" :key="indexX">
      <div
        v-for="(field, indexY) in x"
        :key="indexY"
        @click="addBuilding(indexX, indexY)"
      >
        <BuildingBackground :field="field">
          <component :is="field.type" :buildingData="field"></component>
        </BuildingBackground>
      </div>
    </div>
  </div>
</template>

<script>
import House from "@/components/Buildings/House.vue";
import BuildingBackground from "@/components/Ui/BuildingBackground.vue";
import { BUILDING_TYPES } from "./Buildings/constants";
import buildingMethodsMixin from "../mixins/buildingMethodsMixin";

export default {
  name: "Map",
  components: {
    House,
    BuildingBackground
  },
  mixins: [buildingMethodsMixin],
  data() {
    return {
      mapSizeX: 15,
      mapSizeY: 15,
      gameMap: null
    };
  },
  methods: {
    createMap() {
      return Array(this.mapSizeX)
        .fill(0)
        .map(() => Array(this.mapSizeY).fill({ empty: true }));
    },
    addBuilding(x, y) {
      const newLine = this.gameMap[x];
      const building = {
        type: BUILDING_TYPES.HOUSE,
        health: 0,
        maxHealth: 500
      };

      building.buildTimer = this.initiateBuilding(building);

      newLine[y] = building;

      this.$set(this.gameMap, x, [...newLine]);
    }
  },
  created() {
    this.gameMap = this.createMap();
  }
};
</script>

<style scoped lang="scss">
@import '../styles/mixins.scss';
@import '../styles/variables.scss';

.map {
  @include flexCenter;
  width: $mapSize;
  height: $mapSize;
}
</style>
