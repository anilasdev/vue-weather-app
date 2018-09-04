
<template>
<div class="weather-container">
 <div v-for="(data,index) in weather.forecast.simpleforecast.forecastday" v-bind:key="index" class="weather-wrapper"> 
<img v-bind:src="data.icon_url" class="weather-icon"/>
<div class="temparature">
  <div>{{data.high.celsius}} &#176;C <span class="temperature-arrows">&uarr;</span> </div>
  <div>{{data.low.celsius}} &#176;C <span class="temperature-arrows">&darr;</span> </div>
</div>
</div>
</div>
</template>
<script>
/* eslint-disable */
export default {
  name: "Weather",
  props: ["weatherData"],
  data: function() {
    return {
      weather: this.weatherData
    };
  },
  watch: {
    weatherData: function(newVal, oldVal) {
      this.weather = newVal;
    }
  }
};
</script>

<style scoped>
.weather-wrapper {
  width: 100%;
  height: 30vh;
  border: 1px solid lightblue;
  margin: 10px;
}
.weather-icon {
  float: right;
  margin-right: 30px;
  margin-top: 30px;
}
.temparature {
  display: grid;
  margin-top: 5vh;
  font-size: 30px;
  font-weight: bold;
}
.temperature-arrows {
  font-size: 20px;
}
@media (min-width: 451px) and (max-width:900px){
 .weather-container {
  display: flex;
  width: 100%;
  flex-wrap: wrap;
  justify-content: center;
}
.weather-wrapper{
  width:45%;
}
}
@media (min-width: 0) and (max-width:450px){
 .weather-container {
  display: flex;
  width: 100%;
  flex-wrap: wrap;
  justify-content: center;
}
.weather-wrapper{
  width:100%;
}
}
.weather-container {
  display: flex;
  width: 100%;
  justify-content: center;
}

</style>

