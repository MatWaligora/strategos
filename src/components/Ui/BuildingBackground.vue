<template>
  <div :class="['field', { building: !!field.type }, buildingLifeClass(field)]">
    <slot></slot>
  </div>
</template>

<script type="text/babel">
export default {
  name: "BuildingBackground",
  props: {
    field: Object
  },
  methods: {
    buildingLifeClass({ health, maxHealth }) {
      let result = "good";
      switch (true) {
        case health === 0: {
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
  &.building {
    background-color: red;
    &.good {
      background-color: lightgreen;
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
