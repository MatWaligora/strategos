<template>
  <div :class="['field', { building: !!field.type }]">
    <div
      class="progress"
      :class="buildingLifeClass(field)"
      :style="{ width: lifePercent }"
    ></div>
    <slot></slot>
  </div>
</template>

<script type="text/babel">
export default {
  name: "BuildingBackground",
  props: {
    field: Object
  },
  computed: {
    lifePercent() {
      const { health, maxHealth } = this.field;
      return `${(health / maxHealth) * 100}%`;
    }
  },
  methods: {
    buildingLifeClass({ health, maxHealth }) {
      if (this.field.empty) {
        return "";
      }
      let result = "good";
      switch (true) {
        case health < maxHealth * 0.2: {
          result = "destroyed";
          break;
        }
        case health < maxHealth * 0.8: {
          result = "damaged";
          break;
        }
        default:
          result = "good";
      }
      return result;
    }
  }
};
</script>

<style lang="scss" scoped>
@import "../../styles/mixins.scss";
@import "../../styles/variables.scss";

.field {
  @include flexCenter;
  width: $fieldSize;
  height: $fieldSize;
  border: 1px solid black;
  position: relative;
  .progress {
    position: absolute;
    height: 5px;
    top: 0;
    left: 0;
    background-color: #fff;
    &.good {
      background-color: lightgreen;
    }
    &.damaged {
      background-color: orange;
    }
    &.destroyed {
      background-color: red;
    }
  }
}
</style>
