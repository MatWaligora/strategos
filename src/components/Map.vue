<template>
  <div class="map">
    <div v-for="(x, indexX) in gameMap" :key="indexX">
      <div
        v-for="(field, indexY) in x"
        :key="indexY"
        class="field"
        :class="[field.type, field.type === 'building' ? buildingLifeClass(field) : '']"
        @click="addBuilding(indexX, indexY)"
      >
        {{field.health}}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Map",
  data() {
    return {
      mapSizeX: 15,
      mapSizeY: 15,
      gameMap: null
    };
  },
  methods: {
    createMap() {
      const gameMap = [];
      for (let x = 0; x < this.mapSizeX; x++) {
        gameMap[x] = [];
        for (let y = 0; y < this.mapSizeY; y++) {
          gameMap[x][y] = {
            empty: true
          };
        }
      }
      return gameMap;
    },
    addBuilding(x, y) {
      console.log(x, y, this.gameMap[x]);
      const newLine = this.gameMap[x];
      const building = {
        type: "building",
        health: 0,
        maxHealth: 500,
      };

      building.buildTimer = this.initiateBuilding(building);

      newLine[y] = building;

      this.$set(this.gameMap, x, [...newLine]);
    },
    initiateBuilding(building) {
      building.timer = setInterval(() => {
        building.health += 50;
        if(building.health >= building.maxHealth) {
          building.health = building.maxHealth;
          clearInterval(building.timer);
        }
      }, 1000);
      return building;
    },
    buildingLifeClass ({health, maxHealth}) {
      console.log({health, maxHealth});
      let result = 'good';
      switch (true) {
        case health < (maxHealth * 0.8): {
          result = 'damaged';
          break;
        }
        case health === 0: {
          result = 'destroyed';
          break;
        }
        default: result = 'good'
      }
      return result;
    }
  },
  created() {
    this.gameMap = this.createMap();
  }
};
</script>

<style scoped lang="scss">
$fieldSize: 50px;
$mapSize: 90%;

@mixin flexCenter() {
  display: flex;
  justify-content: center;
  align-items: center;
}

.map {
  @include flexCenter;
  width: $mapSize;
  height: $mapSize;
}

.field {
  @include flexCenter;
  width: $fieldSize;
  height: $fieldSize;
  border: 1px solid black;
  &.building {
    background-color: red;
    &.good {
      background-color: green;
    }
    &.damaged {
      background-color: yellow;
    }
    &.destroyed {
      background-color: red;
    }
  }
}
</style>
