<template>
  <div>
    <h2>Places Today</h2>
    <div>
      <label for="sort-select">Sort by:</label>
      <select id="sort-select" v-model="sortOption" @change="sortPlaces">
        <option value="temperature_asc">Temperature (Asc)</option>
        <option value="temperature_desc">Temperature (Desc)</option>
        <option value="humidity_asc">Humidity (Asc)</option>
        <option value="humidity_desc">Humidity (Desc)</option>
        <option value="wind_asc">Wind Speed (Asc)</option>
        <option value="wind_desc">Wind Speed (Desc)</option>
      </select>
    </div>
    <div>
      <label for="city-input">Add City:</label>
      <input id="city-input" type="text" v-model="newCity" />
      <button @click="addCity">Add</button>
    </div>
    <table class="weather-table">
      <thead>
        <tr>
          <th>City</th>
          <th>Temperature (°C)</th>
          <th>Humidity (%)</th>
          <th>Wind Speed (km/h)</th>
          <th>Weather</th>
          <th>Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(place) in sortedPlaces" :key="place.name">
          <td>{{ place.name }}</td>
          <td>{{ place.temperature }}</td>
          <td>{{ place.humidity }}</td>
          <td>{{ place.wind }}</td>
          <td>{{ place.weather }}</td>
          <td>
            <button @click="removeCity(place)">Remove</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'HottestPlaces',
  data() {
    return {
      hottestPlaces: [],
      sortOption: 'temperature_desc',
      newCity: '',
    };
  },
  mounted() {
    this.loadSavedCities();
    this.fetchWeatherInfo();
  },
  computed: {
    sortedPlaces() {
      const sorted = [...this.hottestPlaces];
      if (this.sortOption === 'temperature_asc') {
        sorted.sort((a, b) => a.temperature - b.temperature);
      } else if (this.sortOption === 'temperature_desc') {
        sorted.sort((a, b) => b.temperature - a.temperature);
      } else if (this.sortOption === 'humidity_asc') {
        sorted.sort((a, b) => a.humidity - b.humidity);
      } else if (this.sortOption === 'humidity_desc') {
        sorted.sort((a, b) => b.humidity - a.humidity);
      } else if (this.sortOption === 'wind_asc') {
        sorted.sort((a, b) => a.wind - b.wind);
      } else if (this.sortOption === 'wind_desc') {
        sorted.sort((a, b) => b.wind - a.wind);
      }
      return sorted;
    },
  },
  methods: {
    fetchWeatherInfo() {
      const apiKey = 'dc67789a08d491e885529249616ed9b8';
      const promises = this.hottestPlaces.map((place) =>
        axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${place.name}&units=metric&appid=${apiKey}`)
      );

      Promise.all(promises)
        .then((responses) => {
          responses.forEach((response, index) => {
            const { main, wind, weather } = response.data;
            this.hottestPlaces[index].temperature = main.temp;
            this.hottestPlaces[index].humidity = main.humidity;
            this.hottestPlaces[index].wind = Math.round(wind.speed * 3.6);
            this.hottestPlaces[index].weather = weather[0].description;
          });
        })
        .catch((error) => {
          console.error(error);
        });
    },
    addCity() {
  const city = this.newCity.trim();
  if (city === '') return;

  const existingCity = this.hottestPlaces.find(
    (place) => place.name.toLowerCase() === city.toLowerCase()
  );
  if (existingCity) {
    alert('City already exists in the list.');
    return;
  }

  // Check if the city exists
  const apiKey = 'dc67789a08d491e885529249616ed9b8';
  axios
    .get(`https://api.openweathermap.org/data/2.5/weather?q=${city}&units=metric&appid=${apiKey}`)
    .then((response) => {
      const { main, wind, weather } = response.data;
      this.hottestPlaces.push({
        name: city,
        temperature: Math.round(main.temp), // Convert from Kelvin to Celsius
        humidity: main.humidity,
        wind: Math.round(wind.speed * 3.6),
        weather: weather[0].description,
      });
      this.newCity = '';
      this.saveCities();
    })
    .catch((error) => {
      alert('Invalid city. Please enter a valid city name.');
      console.error(error);
    });
},
    removeCity(place) {
      this.hottestPlaces = this.hottestPlaces.filter((p) => p !== place);
      this.saveCities();
    },
    saveCities() {
      localStorage.setItem('hottestPlaces', JSON.stringify(this.hottestPlaces));
    },
    loadSavedCities() {
      const savedPlaces = localStorage.getItem('hottestPlaces');
      if (savedPlaces) {
        this.hottestPlaces = JSON.parse(savedPlaces);
      } else {
        // Add default cities if cache is empty
        this.hottestPlaces = [
          { name: 'London', temperature: '', humidity: '', wind: '', weather: '' },
          { name: 'Paris', temperature: '', humidity: '', wind: '', weather: '' },
          { name: 'Tokyo', temperature: '', humidity: '', wind: '', weather: '' },
          { name: 'Berlin', temperature: '', humidity: '', wind: '', weather: '' },
          { name: 'New York', temperature: '', humidity: '', wind: '', weather: '' },
          { name: 'Casablanca', temperature: '', humidity: '', wind: '', weather: '' },
          { name: 'Shanghai', temperature: '', humidity: '', wind: '', weather: '' },
        ];
        this.saveCities(); // Save the default cities to cache
      }
    },
    sortPlaces() {
      // Trigger the computed property to recompute the sorted places
    },
  },
};
</script>

<style scoped>
.weather-table {
  width: 100%;
  table-layout: fixed;
  border-collapse: collapse;
}

.weather-table th,
.weather-table td {
  padding: 10px;
  text-align: center;
}

.weather-table thead th {
  background-color: #3498db;
  color: #fff;
}

.weather-table tbody td {
  background-color: #fff;
  color: #333;
}

.weather-table tbody tr:nth-child(odd) td {
  background-color: #f2f2f2;
}

.weather-table tbody tr:hover td {
  background-color: #e0e0e0;
}

.weather-table tbody tr td:first-child {
  font-weight: bold;
}

.weather-table td:last-child {
  position: relative;
}

.weather-table td[data-temperature^='-']::after {
  color: #2980b9;
}

.weather-table td[data-temperature^='+']::after {
  color: #e67e22;
}
</style>
