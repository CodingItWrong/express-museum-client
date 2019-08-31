<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png" />
    <RestaurantList />
  </div>
</template>

<script>
// @ is an alias to /src
import RestaurantList from "@/components/RestaurantList.vue";

export default {
  name: "home",
  components: {
    RestaurantList
  }
};
</script>
