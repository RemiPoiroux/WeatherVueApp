<template>
    <footer class="weather-footer">
      <div class="weather-footer__container">
        <p class="weather-footer__text">Weather App &copy; May 2023 By Rémi Poiroux</p>
      </div>
    </footer>
  </template>
  
  <script>
  export default {
    name: 'WeatherFooter'
  };
  </script>
  
  <style scoped>
.weather-footer {
  background-color: #2c3e50;
  padding: 20px;
  color: #fff;
  font-size: 14px;
  width: 100%;
}

.weather-footer__container {
  display: flex;
  justify-content: center;
  align-items: center;
  max-width: 1200px;
  margin: 0 auto;
}

.weather-footer__text {
  margin: 0;
}
</style>
  