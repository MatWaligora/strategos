<template>
  <div class="home">
    <Map />
  </div>
</template>

<script>
// @ is an alias to /src
import Map from "@/components/Map.vue";

export default {
  name: "home",
  components: {
    Map
  }
};
</script>

<style>
.home {
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>