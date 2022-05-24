<template>
  <header>
    <img src="./../assets/images/logo.svg" alt="" />
    <FilterForm v-bind:cities="cities" />
  </header>
</template>

<script>
import FilterForm from "./FilterForm.vue";

export default {
  name: "HeaderComponent",
  props: ["cities"],
  components: {
    FilterForm,
  },
};
</script> 