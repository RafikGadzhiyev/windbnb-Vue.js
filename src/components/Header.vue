<template>
  <header>
    <img src="./../assets/images/logo.svg" alt="" />
    <FilterForm v-bind:cities="cities" v-bind:stays = "stays" :filterMethod="filterMethod" />
  </header>
</template>

<script>
import FilterForm from "./FilterForm.vue";

export default {
  name: "HeaderComponent",
  props: ["cities", "stays", "filterMethod"],
  components: {
    FilterForm,
  },
};
</script> 

<style>
  header{
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding-top: 20px;
  }
</style>