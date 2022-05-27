<template>
  <div 
    class="form_content-container" 
    v-bind:class="{ active_container: isOpened }" 
    v-on:click = "closeFilter"
    >
    <div class="form-wrapper" v-bind:class="{ active_wrapper: isOpened }">
    <div class="filter_description-container">
      <span class = 'description'>Edit your search</span>
      <button class="close_filter-button" v-on:click = "closeFilterByButton">
        <i class="bi bi-x-lg"></i>
      </button>
    </div>
      <form 
      v-on:submit.prevent class="form" 
      v-on:click = "check"
      >
        <div 
          class="city-filter" 
          v-on:click="activate('city')" 
          v-bind:class = "{bordered: currentFocus === 'city'}"
        >
          <span class="filter-name" v-bind:class = "{disabled: !isOpened}">Location</span>
          <input
            type="text"
            placeholder="Add location"
            disabled
            v-bind:value="CityFilterValue"
            class="filter-input"
          />
        </div>
        <div 
          class="guests-filter"  
          v-on:click="activate('guests')"
          v-bind:class = "{bordered: currentFocus === 'guests'}"
        >
          <span class="filter-name" v-bind:class = "{disabled: !isOpened}">Guests</span>
          <input
            type="text"
            placeholder="Add guests"
            v-bind:value="GuestsFilterValue"
            disabled
            class="filter-input"
          />
        </div>
        <button class="search_by_filters-button" v-on:click="activeButton">

          <i class="bi bi-search"></i>
          <span>Search</span>
          </button>
      </form>
      <div class="filter_variants-container">
        <div
          class="city_variants-container"
          v-bind:class="{ active: currentFocus === 'city' }"
        >
          <button
            class="city"
            v-for="city in cities"
            v-bind:key="city"
            v-on:click="SetChosenCity(city)"
          >
            <i class="bi bi-geo-alt-fill"></i>
            {{ city }}
          </button>
        </div>
        <div
          class="guests_params-container"
          v-bind:class="{ active: currentFocus === 'guests' }"
        >
          <div class="adults">
            <span class="param-value">Adults</span>
            <span class="param-description">Ages 13 or above</span>
            <div class="adults-counter counter">
              <button
                class="dicrement-button"
                v-on:click="DecrementCounter('Adults')"
              >
                <i class="bi bi-dash"></i>
              </button>
              <span class="current_counter-value">{{ totalAdults }}</span>
              <button
                class="increment-button"
                v-on:click="IncrementCounter('Adults')"
              >
                <i class="bi bi-plus-lg"></i>
              </button>
            </div>
          </div>
          <div class="children">
            <span class="param-value">Children</span>
            <span class="param-description">Ages 2-12</span>
            <div class="children-counter counter">
              <button
                class="dicrement-button"
                v-on:click="DecrementCounter('Children')"
              >
                <i class="bi bi-dash"></i>
              </button>
              <span class="current_counter-value">{{ totalChildren }}</span>
              <button
                class="increment-button"
                v-on:click="IncrementCounter('Children')"
              >
                <i class="bi bi-plus-lg"></i>
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import "bootstrap-icons/font/bootstrap-icons.css";
export default {
  name: "FilterForm",
  components: {},
  props: ["cities", "stays", "filterMethod"],
  data() {
    return {
      CityFilterValue: "",
      GuestsFilterValue: "",
      totalAdults: 0,
      totalChildren: 0,
      currentFocus: "",
      isOpened: false
    };
  },
  methods: {
    IncrementCounter(counterType) {
      switch (counterType) {
        case "Adults":
          this.totalAdults++;
          break;
        case "Children":
          this.totalChildren++;
          break;
        default:
          break;
      }
      this.setCurrentGuestTotal();
    },
    DecrementCounter(counterType) {
      switch (counterType) {
        case "Adults":
          this.totalAdults--;
          if (this.totalAdults < 0) {
            this.totalAdults = 0;
          }
          break;
        case "Children":
          this.totalChildren--;
          if (this.totalChildren < 0) {
            this.totalChildren = 0;
          }
          break;
        default:
          break;
      }
      this.setCurrentGuestTotal();
    },
    setCurrentGuestTotal() {
      let total = this.totalAdults + this.totalChildren;
      this.GuestsFilterValue =
        total === 0 ? "" : `${total} guest${total > 1 ? "s" : ""}`;
    },
    SearchByFilters() {},
    SetChosenCity(cityValue) {
      this.CityFilterValue = cityValue;
    },
    setFocus(focusName) {
      this.currentFocus = focusName;
    },
    resetFocus() {
      this.currentFocus = "";
    },
    activate(filterName){
      this.setFocus(filterName)
  this.isOpened = true;
    },
    closeFilter(e){
      let element = e.target;
      if(element.classList.contains("active_container")){
        this.isOpened = false;
        this.setFocus('');
      }
    },
    updateData(){
      if(!this.isOpened) return this.isOpened = true;
      this.filterMethod({
        cityFilter: this.CityFilterValue,
        guestsFilter: this.totalAdults + this.totalChildren
      })
      this.CityFilterValue = '';
      this.GuestsFilterValue = '';
      this.totalAdults = 0;
      this.totalChildren = 0;
      this.isOpened = false;
  },
    activeButton(){
      this.updateData();
},
closeFilterByButton(){
  this.isOpened = false;
}
  },
};
</script>
<style>
.form_content-container{
  width: 90%;
}
.active_container {
  width: 100%;
  height: 100vh;
  position: fixed;
  z-index: 10;
  top: 0;
  left: 0;
  background: rgba(0, 0, 0, 0.5);
}

.active_wrapper {
  width: 100%;
  min-height: 40%;
  max-height: 80vh;
  background: #fff;
  padding: 50px 100px;
}

.form-wrapper{
  width: 100%;
  display: flex;
  justify-content: flex-end;
}

.filter_description-container{
  display: none;
}



.close_filter-button{
  all: unset;
  cursor: pointer;
  font-weight: 700;
  color: #828282;
  transition: 100ms ease;
}

.description{
  font-weight: 700;
}

.close_filter-button:hover{
  color: black;
}

.form{
  width: fit-content;;
  box-shadow: 0 1px 6px 0 rgb(189, 189, 189);
  border-radius: 10px;
  overflow: hidden;
}

.active_wrapper{
  display: block;
}

.active_wrapper > .form {
  width: 100%;
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  place-items: center;
  box-shadow: 0 1px 6px rgba(224, 224, 224, 0.856);
  border-radius: 10px;
  margin-bottom: 25px;
}

.city-filter,
.guests-filter{
  display: inline-block;
  width: fit-content;
  cursor: pointer;
  padding: .75rem 1rem;
}


.guests-filter{
  border-inline: 1px solid #f2f2f2;
}

.filter_variants-container{
  display: none;
}

.active_wrapper .city-filter,
.active_wrapper .guests-filter {
  width: 100%;
  border: 1px solid #f2f2f2;
  padding: 10px 30px;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  gap: 5px;
}

.filter-name {
  font-size: 14px;
  font-weight: 700;
  text-transform: capitalize;
}

.param-value {
  font-weight: 700;
  text-transform: capitalize;
}
.active_wrapper .city-filter {
  border-radius: 10px 0 0 10px;
}

.filter-input {
  all: unset;
}

.search_by_filters-button{
  all: unset;
  width: 20px;
  overflow: hidden;
  white-space: pre;
  padding: 12px 20px;
  color: #eb5757;
  cursor: pointer;
}

.search_by_filters-button > i{
  margin-right: .5rem;
}

.search_by_filters-button > span{
  display: none;
}

.active_wrapper .search_by_filters-button > span{
  display: inline;
}

.active_wrapper  .search_by_filters-button {
  all: unset;
  width: 20%;
  background-color: #eb5757;
  color: white;
  padding: 10px;
  text-align: center;
  border-radius: 10px;
  min-width: 100px;
  transition: 300ms ease;
  margin-left: 1rem;
}
.active_wrapper  .search_by_filters-button:hover {
  background-color: #f17070;
}

.active_wrapper .filter_variants-container {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 20px;
}
.city_variants-container {
  display: flex;
  flex-direction: column;
  gap: 20px;
}
.city {
  all: unset;
  cursor: pointer;
  padding: 10px 10px;
  border-radius: 10px;
  transition: 300ms ease;
}

.city:hover {
  background-color: #eee;
}

.adults,
.children {
  display: flex;
  flex-direction: column;
  margin-bottom: 25px;
}

.param-description {
  color: #c1c1c1;
  font-size: 14px;
  font-weight: 100;
}
.counter {
  margin-top: 15px;
}

.dicrement-button,
.increment-button {
  all: unset;
  cursor: pointer;
  border-radius: 5px;
  border: 1px solid #828282;
  color: #828282;
  transition: 200ms ease;
}

.dicrement-button {
  margin-right: 10px;
}

.increment-button {
  margin-left: 10px;
}

.dicrement-button:hover,
.increment-button:hover {
  color: black;
  border-color: black;
}

.city_variants-container,
.guests_params-container {
  display: none;
}

.guests_params-container {
  grid-column-start: 2;
}

.active {
  display: flex;
  flex-direction: column;
}

.active_wrapper .bordered{
  border-color: black ;
}

.disabled{
  display: none;
}

@media screen and (max-width: 825px) {
  .form_content-container{
    width: 100%;
  }
  .form-wrapper{
    justify-content: center;
  }

  .form{
    width: fit-content;
    display: grid;
    grid-template-columns: repeat(2, 1fr) .5fr;
  }

  .city-filter,
  .guests-filter,
  .search_by_filters-button{
    width: 100%;
  }

  .filter-input{
    width: 100%;
  }
}

@media screen and (max-width: 950px){
  .active_wrapper{
    padding: 50px;
  }
}

@media screen and (max-width: 600px) {
  .active_wrapper{
    padding: 25px 25px 75px 25px;
    position: relative;
  }

  .active_wrapper .filter_description-container {
    display: flex;
    align-items: flex-start;
    height: 30px;
    justify-content: space-between;
  }

  .active_wrapper .search_by_filters-button{
    position: absolute;
    bottom:  1rem;
  }

  .active_wrapper .form{
    grid-template-rows: repeat(2, 1fr);
    grid-template-columns: 1fr;
  }
  .active_wrapper .filter_variants-container{
    display: block;
    padding-left: 10px;
  }
  .active_wrapper .city-filter{
    border-radius: 10px 10px 0 0;
  }
  .active_wrapper .guests-filter{
    border-radius: 0 0 10px 10px;
  }
}

@media screen and (max-height: 600px) {
  .active_wrapper{
    overflow: auto;
  }
}

</style>

