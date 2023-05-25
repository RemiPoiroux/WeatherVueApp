<template>
    <WeatherHeader v-model="currentLocation" />
  <WeatherToday/>
  <WeatherForecast/>
  <WeatherMap/>
  <HottestPlaces/>
  <HistoricalWeather/>
  <WeatherFooter/>
</template>

<script>
import WeatherToday from './components/WeatherToday.vue'
import WeatherMap from './components/WeatherMap.vue'
import WeatherForecast from './components/WeatherForecast.vue';
import WeatherHeader from './components/WeatherHeader.vue'
import WeatherFooter from './components/WeatherFooter.vue'
import HottestPlaces from './components/HottestPlaces.vue'
import HistoricalWeather from './components/HistoricalWeather.vue'

export default {
  name: 'App',
  components: {
    WeatherToday,
    WeatherMap,
    WeatherHeader,
    WeatherFooter,
    HottestPlaces,
    WeatherForecast,
    HistoricalWeather
  },
  data() {
    return {
      currentLocation: 'London', // Initial default location
      isLocationModalOpen: false

    };
  },
  showLocationModal(newLocation) {
      // Implement the logic to show the location selection modal or navigate to a new route for location selection
      // Here, we'll simply prompt the user for the location for demonstration purposes

      this.currentLocation = newLocation;
      this.isLocationModalOpen = true;
    
      }
}
</script>

<style>
#app {
  background-color: #f2f2f2;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  font-family: 'Roboto', sans-serif;
}

.weather-header {
  background-color: #3498db;
  padding: 20px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
  width: 100%;
}

.weather-header__container {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.weather-header__title {
  font-family: 'Montserrat', sans-serif;
  font-size: 32px;
  color: #fff;
  margin: 0;
}
.weather-header__location {
  display: flex;
  align-items: center;
}

.weather-header__location-text {
  font-size: 18px;
  color: #fff;
  margin-right: 10px;
}

.weather-header__change-location {
  background-color: transparent;
  border: none;
  color: #fff;
  font-size: 18px;
  cursor: pointer;
  text-decoration: underline;
  transition: color 0.3s ease;
}

.weather-header__change-location:hover {
  color: #ffd700;
}
</style>