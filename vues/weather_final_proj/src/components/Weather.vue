<template>
  <div class="weather-container">
    <Loading v-if="loading" />
    <div class="weather" :style="{ backgroundImage: weatherBackground }"  style="display: flex;  box-shadow: 0px 0px 10px 5px rgba(0, 0, 0, 0.5); border-radius: 20px">
  
      <div id="div1">
        <br>

        <p style="font-size: 70px; color: black; font-family: 'Rockwell'; ">{{ cityName }}</p>


        <img
        v-if="weather"
        :src="`http://openweathermap.org/img/wn/${weather.icon}.png`"
        alt="Weather Icon"
        class="weather-icon"
        style="height: 70px; width: 70px;"
      />
        <br><br><p v-if="weather"><strong>Opis pogody:</strong> {{ weather.description }}</p><br>
        <p v-if="weatherData">Obecna temperatura: {{ currentTemperature }}</p><br><br><br><br>
        <button @click="toggleTemperatureUnit">Zmiana jednostki</button>
      </div>
      
      <div style="width: 50%;">
        <div style="height: 30px;"></div>
      <div id="div2">
        <div style="display: flex;">
        <img src="../../resilience.png" style="height: 30px; width: 30px"><div style="width: 10px"></div><p v-if="weatherData">Ciśnienie: {{ weatherData.main.pressure }} hPa</p>
      </div>
      <div style="display: flex;">
        <img src="../../humidity.png" style="height: 30px; width: 30px"><div style="width: 10px"></div>
        <p v-if="weatherData">Wilgoć: {{ weatherData.main.humidity }}%</p>
      </div>
      <div style="display: flex;">
        <img src="../../wind.png" style="height: 30px; width: 30px"><div style="width: 10px"></div>
        <p v-if="windSpeed">Prędkość wiatru: {{ windSpeed }} m/s</p>
        </div>
      </div>
      <br>
      <div id="div3">
        <p v-if="sunriseTime">Godzina wschodu słońca:<br> {{ sunriseTime }}</p>
        <p v-if="sunsetTime">Godzina zachodu słońca:<br> {{ sunsetTime }}</p>
      </div>
    </div>
      
    </div>
    
  </div>
</template>

<script setup>
import { ref, computed, defineProps, defineEmits } from 'vue'; 
import Loading from '../components/Loading.vue';
const props = defineProps({
  cityName: String,
  weatherData: Object,
  units: String,
  loading: Boolean,
  toggleTemperatureUnit: Function 
});
const emits = defineEmits(['toggleTemperature']);
const weather = computed(() => props.weatherData && props.weatherData.weather ? props.weatherData.weather[0] : null);
const windSpeed = computed(() => props.weatherData && props.weatherData.wind ? props.weatherData.wind.speed : null);
const sunriseTime = computed(() => props.weatherData && props.weatherData.sys ? formatTime(props.weatherData.sys.sunrise) : null);
const sunsetTime = computed(() => props.weatherData && props.weatherData.sys ? formatTime(props.weatherData.sys.sunset) : null);

const weatherBackground = computed(() => {
  let backgroundUrl = "url('default-background-image.jpg')";
  if (weather.value && weather.value.main) {
    const mainWeather = weather.value.main.toLowerCase();
    switch (mainWeather) {
      case "clear":
        backgroundUrl = "url('https://i1.wp.com/kokomansion.com/wp-content/uploads/2016/12/Sunny-Weather.jpg?fit=422%2C238&ssl=1')";
        break;
      case "clouds":
        backgroundUrl = "url('https://t4.ftcdn.net/jpg/05/13/26/73/360_F_513267391_QEmNGeOFLLqrILTnoq21dReUPp5UsoNr.jpg')";
        break;
      case "rain":
      case "drizzle":
        backgroundUrl = "url('https://media.istockphoto.com/id/503284599/photo/rainy-weather.jpg?s=612x612&w=0&k=20&c=pV38CVp0CLArYEZ6OUWnaqo6J5mo4JpbEZd61Vxr_I4=')";
        break;
      case "thunderstorm":
        backgroundUrl = "url('https://t3.ftcdn.net/jpg/06/50/61/86/360_F_650618637_6Q0dYJvn2q3crrjy7LvvZVsSTnl9JMnG.jpg')";
        break;
      default:
        break;
    }
  }
  return backgroundUrl;
});
const toggleTemperatureUnit = () => {
  emits('toggleTemperature');
};

const currentTemperature = computed(() => {
  if (props.weatherData) {
    const temperature = props.weatherData.main.temp;
    if (props.units === 'metric') {
      return `${temperature}°C`;
    } else {
      return `${temperature}°F`;
    }
  } else {
    return '';
  }
});

const unitSymbol = computed(() => props.units === 'metric' ? 'C' : 'F');

const formatTime = (timestamp) => {
  const date = new Date(timestamp * 1000);
  return date.toLocaleTimeString();
};
</script>

<style>
.weather {
  border-radius: 20px;
  color: black;
  text-align: center;
  width: 100%;
  height: 600px;
  margin-left: 0;
  background-size: cover;
  background-repeat: no-repeat;
}
#div1{
  width: 45%;
  height: 90%;
  background-color: rgba(255, 255, 255, 0.498); 
  margin: auto;
  border-radius: 20px;
  font-size: 20px;
}
#div2{
  width: 90%;
  height: 40%;
  background-color: rgba(255, 255, 255, 0.498); 
  margin: auto;
  border-radius: 20px;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 25px;
  text-align: center;
  flex-direction: column;
  color: black;
}
#div3{
  width: 90%;
  height: 40%;
  background-color: rgba(255, 255, 255, 0.498); 
  margin: auto;
  border-radius: 20px;
  display: flex;
  justify-content: space-around;
  align-items: center;
  font-size: 25px;
  text-align: center;
  flex-direction: column;
  color: black;
  padding-top: 2rem;
  padding-bottom: 2rem;
}

</style>
