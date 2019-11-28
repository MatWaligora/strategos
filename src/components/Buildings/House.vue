<template>
  <div>
    {{ buildingData.health }}
  </div>
</template>

<script>
import buildingMethodsMixin from "../../mixins/buildingMethodsMixin";

export default {
  name: "House",
  props: {
    buildingData: Object
  },
  mixins: [buildingMethodsMixin],
  data() {
    return {};
  },
  methods: {},
  created() {}
};
</script>

<style scoped></style>
