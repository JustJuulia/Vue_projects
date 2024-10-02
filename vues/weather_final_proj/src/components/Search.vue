<template>
  <div class="allin">
    <div style="height: 20px;" ></div>
    <div class="search-container">
    <input v-model="city" @keyup.enter="searchCity" placeholder="   Podaj miasto..." />
    <button @click="searchCity" >Wyszukaj</button>
  </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import { defineEmits } from 'vue';

const city = ref('');
const emit = defineEmits(['city-searched']);

const searchCity = async () => {
  if (!city.value) return;
  try {
    const response = await fetch(`https://api.openweathermap.org/data/2.5/weather?q=${city.value}&appid=ab3ce49fefd88f7d1913f3272bb48a09&units=metric&lang=pl`);
    const data = await response.json();
    if (data.cod === "404") {
      alert("City not found!");
      return;
    }
    emit('city-searched', city.value); // Emit the custom event with the city name
    city.value = ""; // Clear the input field
  } catch (error) {
    console.error("Error fetching weather data:", error);
  }
};
</script>

<style>
.search-container {
  margin-top: 20px;
  display: flex;
  justify-content: center;
}

input {
  padding: 10px;
  padding-bottom: 10px;
  font-size: 16px;
  border: 1px solid #ccc;
  border-radius: 5px;
  margin-right: 10px;
  width: 90%;
}

button {
  padding: 10px 20px;
  font-size: 16px;
  background-color: #8aa7c6;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}
</style>
