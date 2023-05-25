<template>
  <div>
    <h2>Weather Forecast</h2>
    <div v-if="loading">Loading...</div>
    <div v-else class="forecast-container">
      <div class="scrollable-wrapper">
        <div class="forecast-list">
          <div v-for="forecast in filteredForecasts" :key="forecast.date" class="forecast-item">
            <p>Date: {{ forecast.date }}</p>
            <p>Temperature: {{ forecast.temperature }}°C</p>
            <p>Weather: {{ forecast.weather }}</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'WeatherForecast',
  data() {
    return {
      forecasts: [],
      loading: true
    };
  },
  mounted() {
    this.getWeatherForecast();
  },
  computed: {
    filteredForecasts() {
      const today = new Date();
      today.setHours(0, 0, 0, 0);
      const nextFiveDays = new Date(today);
      nextFiveDays.setDate(today.getDate() + 5);

      const filtered = this.forecasts.filter(forecast => {
        const forecastDate = new Date(forecast.date);
        return forecastDate >= today && forecastDate < nextFiveDays;
      });

      return filtered.slice(0, 5); // Keep only the first 5 elements
    }
  },
  methods: {
    async getWeatherForecast() {
      const apiKey = 'dc67789a08d491e885529249616ed9b8';
      const city = 'Paris';
      const response = await axios.get(`https://api.openweathermap.org/data/2.5/forecast?q=${city}&units=metric&appid=${apiKey}`);

      this.forecasts = response.data.list.map(item => ({
        date: item.dt_txt,
        temperature: item.main.temp,
        weather: item.weather[0].main
      }));

      this.loading = false;
    }
  }
};
</script>

<style scoped>
.forecast-container {
  overflow-x: auto;
}

.scrollable-wrapper {
  display: inline-flex;
}

.forecast-list {
  display: flex;
  justify-content: space-between;
}

.forecast-item {
  flex: 0 0 18%;
  margin: 10px 0;
}
</style>
