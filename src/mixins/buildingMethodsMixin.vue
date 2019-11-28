<script>
export default {
  name: "buildingMethodsMixin",
  methods: {
    initiateBuilding(building) {
      building.timer = setInterval(() => {
        building.health += 50;
        if (building.health >= building.maxHealth) {
          building.health = building.maxHealth;
          clearInterval(building.timer);
        }
      }, 1000);
      return building;
    },
  }
};
</script>
