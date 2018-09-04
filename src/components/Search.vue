
<template>
<div>
<input class="input-box" @input="onPlaceChange" v-model="inputPlace" />
<ol v-if="showList" class="auto-place-list">
  <li v-for="(d,index) in placesList" v-bind:key="index" v-bind:value="d" v-on:click="selectedPlace(index)"> {{d.name}}</li>
  </ol>
</div>
</template>
<script>
/* eslint-disable */
const weatherApiKey = "26f4b9a9fa269181";
const getWeather = async (lat, lng) => {
  let request = await fetch(
    `http://api.wunderground.com/api/${weatherApiKey}/forecast/q/${lat},${lng}.json`
  );
  let response = await request.json();
  return response;
};
export default {
  name: "Search",
  data: function() {
    return {
      placesList: [],
      inputPlace: "Bangalore, India",
      showList: false
    };
  },
  methods: {
    onPlaceChange: async function() {
      this.$emit("update-isDataFetched", false);
      this.$emit("update-isChangingPlace", true);
      this.showList = true
      let request = await fetch(
        `http://autocomplete.wunderground.com/aq?query=${this.inputPlace}`
      );
      let { RESULTS } = await request.json();
      RESULTS = RESULTS.slice(0, 10);
      this.placesList = RESULTS.map(d => ({
        name: d.name,
        lat: d.lat,
        lon: d.lon
      }));
    },
    selectedPlace: async function(index) {
      this.$emit("update-isChangingPlace", false);
      this.showList = false
      this.inputPlace = this.placesList[index].name;
      let lat = this.placesList[index].lat;
      let lon = this.placesList[index].lon;
      this.placesList = [];
      let weatherData = await getWeather(lat, lon);
      this.$emit("update-places", weatherData);
    }
  }
};
</script>

<style scoped>
ol {
  padding: 0;
}
ol li {
  display: grid;
  justify-content: center;
  list-style: none;
  border: 1px solid lightgray;
  padding: 9px;
  width: 30vw;
  margin: 3px;
}
ol li:hover {
  background-color: whitesmoke;
}
*:focus {
  outline: none;
}
.input-box {
  width: 210px;
  height: 35px;
  border: 2px solid powderblue;
  border-radius: 30px;
  text-align: center;
  font-size: 25px;
  margin-bottom: 30px;
}
.auto-place-list {
  display: flex;
  flex-direction: column;
  align-items: center;
}
@media (min-width: 0) and (max-width: 450px) {
  .input-box {
    width: 100%;
    height: 50px;
  }
  ol li{
    width: 100%;
  }
}
@media (min-width: 451px) and (max-width: 900px) {
  .input-box {
    width: 235px;
  }
}
</style>

