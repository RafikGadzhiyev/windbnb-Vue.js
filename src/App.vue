<template>
  <div class="content-container">
    <HeaderComponent v-bind:cities="cities" />
  </div>
</template>

<script>
import HeaderComponent from "./components/Header.vue";
import json from "./assets/api/RoomsData/stays.json";

export default {
  name: "App",
  components: {
    HeaderComponent,
  },
  data() {
    return {
      stays: json,
      cities: Array.from(new Set(json.map((e) => e.city + ", " + e.country))),
    };
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
.content-container {
  width: 100%;
  min-height: 100vh;
  padding-inline: 50px;
}
</style>
