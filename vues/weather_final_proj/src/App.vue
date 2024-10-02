<template>
  <div id="app">
    <header>
      <div class="search-container">
        <Search @city-searched="searchCity" />
      </div>
    </header>
    <br>
    <main>
      <div class="weather-container">
        <Weather
          v-if="cityName"
          :cityName="cityName"
          :weatherData="weatherData"
          :units="units"
          @toggleTemperature="toggleTemperatureUnit"
          :loading="loading"
        />
      </div>
    </main>
    <footer>
      <div id="made-by" style="color: white;">
        Made by:
        <div>
          <img id="logo" src="https://openweathermap.org/themes/openweathermap/assets/img/logo_white_cropped.png" height="50px">
        </div>
      </div>
    </footer>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import Search from './components/Search.vue';
import Weather from './components/Weather.vue';

const cityName = ref('');
const weatherData = ref(null);
const units = ref('metric');
const loading = ref(false);

const searchCity = async (city) => {
  cityName.value = city;
  loading.value = true;
  try {
    const response = await fetch(
      `https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=ab3ce49fefd88f7d1913f3272bb48a09&units=${units.value}&lang=pl`
    );
    if (!response.ok) {
      throw new Error('Nie znaleziono miasta');
    }
    const data = await response.json();
    weatherData.value = data;
  } catch (error) {
    alert('City not found');
  } finally {
    loading.value = false;
  }
};
const toggleTemperatureUnit = () => {
  units.value = units.value === 'metric' ? 'imperial' : 'metric';
  searchCity(cityName.value);
};
</script>

<style>
#app {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}
body{
  background-color: darkgray;
}
header {
  flex: 0 0 auto; 
}

main {
  flex: 1 0 auto; 
}

footer {
  flex: 0 0 auto;
  margin-top: auto;
}

.search-container {
  width: 900px;
  margin: 0 auto;
}

#made-by {
  margin-right: 20px;
}

#logo {
  height: 50px;
}
</style>
