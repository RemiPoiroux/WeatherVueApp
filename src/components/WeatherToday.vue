<template>
  <div>
    <h2>Current Weather</h2>
    <div>
      <label for="cityInput">Enter City:</label>
      <input type="text" id="cityInput" v-model="city" />
      <button @click="getWeatherData">Get Weather</button>
    </div>
    <p v-if="!loading">Temperature: {{ temperature }}°C</p>
    <p v-if="!loading">Weather: {{ weather }}</p>
    <p v-if="loading">Loading...</p>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'WeatherToday',
  data() {
    return {
      temperature: null,
      weather: null,
      loading: true,
      city: ''
    };
  },
  mounted() {
    this.city = localStorage.getItem('lastCity') || ''; // Retrieve last city from cache
    this.getWeatherData();
  },
  methods: {
    async getWeatherData() {
      const apiKey = 'dc67789a08d491e885529249616ed9b8';
      const apiUrl = `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=${apiKey}`;

      try {
        const response = await axios.get(apiUrl);
        this.temperature = response.data.main.temp;
        this.weather = response.data.weather[0].main;
      } catch (error) {
        console.error(error);
      } finally {
        this.loading = false;
      }
    }
  },
  watch: {
    city(newCity) {
      localStorage.setItem('lastCity', newCity); // Save the last entered city in cache
    }
  }
};
</script>