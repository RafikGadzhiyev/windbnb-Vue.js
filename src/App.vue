<template>
  <div class="content-container">
    <HeaderComponent v-bind:cities="cities" v-bind:stays = "stays" :filterMethod="getFilters"/>
    <MainContentComponent v-bind:stays = "stays" />
  </div>
</template>

<script>
import HeaderComponent from "./components/Header.vue";
import MainContentComponent from "./components/MainContent.vue";
import json from "./assets/api/RoomsData/stays.json";

export default {
  name: "App",
  components: {
    HeaderComponent,
    MainContentComponent
  },
  data() {
    return {
      stays: json,
      cities: Array.from(new Set(json.map((e) => e.city + ", " + e.country))),
    };
  },
  methods: {
    getFilters(filter){
      this.updateStays(filter);
},
  updateStays(filter){
    if(!filter.cityFilter && !filter.guestsFilter) return this.stays = json;
    const filteredStays = [];
    for(let stay of json){
      if((stay.city === filter.cityFilter.split(', ')[0] || !filter.cityFilter) && stay.maxGuests >= filter.guestsFilter){
        filteredStays.push(stay);
  }
  this.stays = filteredStays;
}
  }
  }
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
