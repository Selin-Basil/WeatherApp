<script setup lang="ts">
import { BContainer, BFormInput, BInputGroup, BInputGroupText } from 'bootstrap-vue-next'

import 'primeicons/primeicons.css'
import { ref } from 'vue'
type WeatherData = {
  name: string
  main: {
    temp: number
  }
  weather: [
    {
      description: string
    },
  ]
  sys: {
    country: string
  }
}
const apiKey = '414e6f94dc77fa2d95c4304f18b2e831'
const apiUrl = 'https://api.openweathermap.org/data/2.5/weather?units=metric&q='
const city = ref('')
const weatherData = ref<WeatherData | null>(null)

const handleSearch = async () => {
  try {
    const response = await fetch(`${apiUrl}${city.value}&appid=${apiKey}`)
    if (!response.ok) throw new Error('Network response was not ok')

    const data = await response.json()
    weatherData.value = data
    console.log('Weather data:', data)
  } catch (err) {
    console.error('Error:', err)
  }
}
</script>

<template>
  <div class="d-flex justify-content-center align-items-center vh-100 bg-light p-4">
    <BContainer
      fluid="sm"
      class="p-4"
      :style="{
        background: 'linear-gradient(45deg, rgba(84, 207, 212, 0.82), rgb(69 104 150))',
        maxWidth: '500px',
        height: '500px',
        borderRadius: '15px',
      }"
    >
      <h2 class="d-flex justify-content-center mt-4 mb-6">Weather App</h2>
      <BInputGroup class="mt-3 mb-4">
        <template #append>
          <BInputGroupText
            ><strong class="text-primary pi pi-search" @click="handleSearch()"></strong
          ></BInputGroupText>
        </template>
        <BFormInput v-model="city" />
      </BInputGroup>
      <div
        v-if="weatherData"
        class="d-flex flex-column justify-content-center align-items-center mt-4 text-white"
      >
        <h1 class="title">{{ weatherData.main.temp }}°C</h1>
        <h2 class="name-size">{{ weatherData.name }}</h2>
        <h4>{{ weatherData.weather[0].description }}</h4>
      </div>
    </BContainer>
  </div>
</template>

<style scoped>
.title {
  font-size: 45px;
}
.name-size{
  font-size: 30px;
}
</style>
