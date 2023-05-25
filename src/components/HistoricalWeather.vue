<template>
    <div>
      <h2>Historical Weather Data</h2>
      <div v-if="loading">Loading...</div>
      <div v-else>
        <div v-if="error">{{ error }}</div>
        <div v-else>
          <label for="location-select">Select Location:</label>
          <select id="location-select" v-model="selectedLocation" @change="getHistoricalWeather">
            <option v-for="location in locations" :value="location" :key="location.id">
              {{ location.name }}
            </option>
          </select>
          <div v-if="weatherData">
            <p>Location: {{ weatherData.name }}</p>
            <p>Average Temperature: {{ weatherData.averageTemperature }}°C</p>
            <p>Average Humidity: {{ weatherData.averageHumidity }}%</p>
            <p>Average Wind Speed: {{ weatherData.averageWindSpeed }} m/s</p>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    name: 'HistoricalWeather',
    data() {
      return {
        locations: [
          { id: 'london', name: 'London', latitude: 51.5074, longitude: -0.1278 },
          { id: 'paris', name: 'Paris', latitude: 48.8566, longitude: 2.3522 },
          { id: 'new_york', name: 'New York', latitude: 40.7128, longitude: -74.0060 },
          // Add more locations as needed
        ],
        selectedLocation: null,
        weatherData: null,
        loading: false,
        error: null,
      };
    },
    methods: {
      async getHistoricalWeather() {
        if (!this.selectedLocation) return;
  
        this.loading = true;
        this.error = null;
  
        const apiKey = 'dc67789a08d491e885529249616ed9b8';
        const { latitude, longitude } = this.selectedLocation;
  
        // Calculate the timestamp for one year ago from the current time
        const timestamp = Math.floor((Date.now() - 31536000000) / 1000);
  
        try {
          // Fetch the historical weather data for the last year
          const response = await axios.get(
            `https://api.openweathermap.org/data/2.5/onecall/timemachine?lat=${latitude}&lon=${longitude}&dt=${timestamp}&units=metric&appid=${apiKey}`
          );
  
          // Calculate the average values for temperature, humidity, and wind speed
          const weatherEntries = response.data.hourly;
          const totalTemperature = weatherEntries.reduce((sum, entry) => sum + entry.temp, 0);
          const totalHumidity = weatherEntries.reduce((sum, entry) => sum + entry.humidity, 0);
          const totalWindSpeed = weatherEntries.reduce((sum, entry) => sum + entry.wind_speed, 0);
  
          const averageTemperature = totalTemperature / weatherEntries.length;
          const averageHumidity = totalHumidity / weatherEntries.length;
          const averageWindSpeed = totalWindSpeed / weatherEntries.length;
  
          this.weatherData = {
            name: this.selectedLocation.name,
            averageTemperature: averageTemperature.toFixed(2),
            averageHumidity: averageHumidity.toFixed(2),
            averageWindSpeed: averageWindSpeed.toFixed(2),
          };
        } catch (error) {
          this.error = 'Failed to fetch historical weather data.';
          console.error(error);
        } finally {
          this.loading = false;
        }
      },
    },
  };
  </script>
  