<template>
    <div class="weather-card">
          <div :style="backgroundStyle">
            <div class="weather-temperature">{{ temperature }}°</div>
            <div class="weather-city">{{ city }}</div> 
        </div>
        <div class="weather-details">
            <div class="current-date">{{ currentDate }}</div>
            <div class="weather-description">{{ capitalizedDescription }}</div>
        </div>
  </div>
</template>

<script>
import dayjs from 'dayjs'

export default {
  name: 'WeatherCard',
  props: {
    city: String,
    temperature: Number,
    description: String,
    backgroundUrl: String
  },
  data() {
    return {
      currentDate: dayjs().format('dddd, MMMM D')
    }
  },
  computed: {
    backgroundStyle() {
      let gradient = ''
      const temp = this.temperature
       if (temp <= 0) {
        gradient = 'linear-gradient(to right, rgba(58, 96, 115, 0.4), rgba(22, 34, 42, 1))'
      } else if (temp <= 10) {
        gradient = 'linear-gradient(to right, rgba(116, 235, 213, 0.4), rgba(172, 182, 229, 1))'
      } else if (temp <= 20) {
        gradient = 'linear-gradient(to right, rgba(252, 234, 187, 0.4), rgba(248, 181, 0, 1))'
      } else {
        gradient = 'linear-gradient(to right, rgba(242, 112, 156, 0.4), rgba(255, 148, 114, 1))'
      }

      return {
        backgroundImage: `${gradient}, url(${this.backgroundUrl})`,
        backgroundSize: 'cover',
        backgroundPosition: 'center',
        color: 'white',
        padding: '20px',
        borderTopLeftRadius: '25px',
        borderTopRightRadius: '25px',
        minHeight: '239px'
      }
    },
    capitalizedDescription() {
    if (!this.description) return ''
    return this.description.charAt(0).toUpperCase() + this.description.slice(1)
  }
  }
}
</script>

<style scoped>
.weather-card {
    width: 100%;
    max-width: 245px;
    border-radius: 25px;
    box-shadow: 0 4px 8px rgba(0,0,0,0.2);
}
.weather-temperature {
    font-size: 40px;
    font-weight: bold;
}
.weather-city {
    font-size: 15px;
    font-weight: bold;
    text-transform: uppercase;
}
.weather-details {
    padding: 25px
}
.current-date {
    font-size: 15px;
    color: #191919;
    font-weight: bold;
    padding-bottom: 5px;
}
.weather-description {
    color: #6B6B6B;
    font-size: 12px;
    font-weight: bold;
}
</style>
