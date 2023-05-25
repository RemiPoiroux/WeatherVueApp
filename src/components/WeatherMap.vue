<template>
  <div class="world-map">
    <div ref="mapContainer" class="map-container"></div>
  </div>
</template>

<script>
import L from 'leaflet';
import 'leaflet/dist/leaflet.css';
import axios from 'axios';

export default {
  name: 'WeatherMap',
  mounted() {
    this.initializeMap();
    this.fetchTemperatureData();
  },
  methods: {
    initializeMap() {
      this.map = L.map(this.$refs.mapContainer, {
        center: [0, 0],
        zoom: 2,
        zoomControl: false,
        attributionControl: false,
      });

      L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
        maxZoom: 18,
        attribution: 'Map data &copy; OpenStreetMap contributors',
      }).addTo(this.map);
    },
    async fetchTemperatureData() {
  const apiKey = 'dc67789a08d491e885529249616ed9b8';
  const apiUrl = `https://api.openweathermap.org/data/2.5/box/city?bbox=-180,-90,180,90,2&appid=${apiKey}`;

  try {
    const response = await axios.get(apiUrl);
    const { list } = response.data;

    list.forEach(async (place) => {
      const { name } = place;
      const lat = place.coord.Lat;
      const lon = place.coord.Lon;

      const weatherData = await this.fetchWeatherData(lat, lon, apiKey);
      const temperature = weatherData.main.temp;
      const iconCode = weatherData.weather[0].icon;

      const marker = L.marker([lat, lon]).addTo(this.map);
      marker.bindPopup(`${name}<br>Temperature: ${temperature}°C`);
      marker.setIcon(this.createWeatherIcon(iconCode));
    });
  } catch (error) {
    console.error(error);
  }
},
async fetchWeatherData(lat, lon, apiKey) {
  const apiUrl = `https://api.openweathermap.org/data/2.5/weather?lat=${lat}&lon=${lon}&units=metric&appid=${apiKey}`;
  try {
    const response = await axios.get(apiUrl);
    return response.data;
  } catch (error) {
    console.error(error);
    return null;
  }
},
createWeatherIcon(iconCode) {
  return L.icon({
    iconUrl: `https://openweathermap.org/img/w/${iconCode}.png`,
    iconSize: [40, 40],
    iconAnchor: [20, 40],
    popupAnchor: [0, -40],
  });
}
  },
};
</script>

<style scoped>
.world-map {
  width: 100%;
  height: 500px;
}

.map-container {
  width: 100%;
  height: 100%;
}
</style>
