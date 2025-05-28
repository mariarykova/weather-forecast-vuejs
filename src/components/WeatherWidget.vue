
<template>
    <h1>Weather Widget</h1>
    <div class="weather-card-container">
        <template v-for="cityName in cities" :key="cityName">
            <WeatherCard
                :city="cityName"
                :temperature="weatherData[cityName]?.temperature"
                :description="weatherData[cityName]?.description"
                :backgroundUrl="weatherData[cityName]?.backgroundUrl"
                :iconCode="weatherData[cityName]?.iconCode"
            />
            </template>
    </div>
</template>

<script>
import axios from 'axios'
import WeatherCard from './WeatherCard.vue'

export default {
  name: 'WeatherWidget',
  components: {
    WeatherCard
  },
  data() {
    return {
        cities: ['Perth', "Sydney", 'Moscow', 'Paris', 'London', ],
      weatherData: {}
    }
  },
  mounted() {
    this.getWeatherForAllCities()
  },
  methods: {
    async getWeatherForCity(cityName) {
      const apiKey = import.meta.env.VITE_API_KEY_WEATHER
      const unsplashKey = import.meta.env.VITE_API_KEY_UNSPLASH
      const url = `https://api.openweathermap.org/data/2.5/weather?q=${cityName}&units=metric&appid=${apiKey}`
      try {
        const response = await axios.get(url)
        const data = response.data
        const desc = data.weather[0].description
        const bgUrl = await this.getBackgroundUrl(cityName, unsplashKey)
         const iconCode = data.weather[0].icon

        this.weatherData = {
            ...this.weatherData,
            [cityName]: {
                temperature: Math.round(data.main.temp),
                description: data.weather[0].description,
                backgroundUrl: bgUrl,
                iconCode: iconCode
            }
    }
      } catch (error) {
        console.error(`Error fetching weather for ${cityName}:`, error)
      }
    },
    async getBackgroundUrl(query, apiKey) {
      const url = `https://api.unsplash.com/photos/random?query=${query}&orientation=landscape&client_id=${apiKey}`
      try {
        const response = await axios.get(url)
        return response.data.urls.full
      } catch (error) {
        console.error('Error fetching background from Unsplash:', error)
        return 'https://static.everypixel.com/ep-pixabay/0793/0924/7142/98982/7930924714298982621-birds.jpg'
      }
    },
    getWeatherForAllCities() {
      this.cities.forEach(city => {
        this.getWeatherForCity(city)
      })
    }
  }
}
</script>

<style scoped>
.weather-card-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 20px;
}
</style>

