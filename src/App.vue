<template>
  <div id="app">
    <div>
    <h1>Vue Weather App</h1>
    </div>
    <div class="weather"> 
    <Search   @update-places="onUpdatePlaces" @update-isDataFetched ="UpdateDataFetched" @update-isChangingPlace ="UpdateChangingPlace" />
    <Weather v-if="isDataFetched" v-bind:weatherData="weatherData"/>
    <span v-else-if="isChangingPlace"></span>
    <img  class="loading-gif" v-else src="./assets/weather-loading.gif"/>
    </div>
  </div>
</template>

<script>
/* eslint-disable */
import Search from "./components/Search.vue";
import Weather from "./components/Weather.vue";
import weatherLoading from "./assets/weather-loading.gif";

export default {
  name: "app",
  components: {
    Search,
    Weather
  },
  data() {
    return {
      weatherData: {},
      isDataFetched: false,
      isChangingPlace: false
    };
  },
  methods: {
    onUpdatePlaces(data) {
      this.weatherData = data;
      this.UpdateDataFetched(true);
    },
    UpdateDataFetched(bool) {
      this.isDataFetched = bool;
    },
    UpdateChangingPlace(bool) {
      this.isChangingPlace = bool;
    }
  },
  async mounted() {
    let request = await fetch(
      `http://api.wunderground.com/api/26f4b9a9fa269181/forecast/q/12.970000,77.589996.json`
    );
    let response = await request.json();
    this.weatherData = response;
    this.UpdateDataFetched(true);
  }
};
</script>

<style >
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.loading-gif {
  height: 60vh;
  width: 60vw;
}
.weather{
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items:center;
}
@media (min-width: 0) and (max-width:450px){
 #app{
 margin-top: 20px;
}
}
</style>
