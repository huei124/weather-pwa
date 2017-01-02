<template>
  <div class="main">
    <city v-for="city in cities" :city="city" class="card weather-forecast"></forecast>
  </div>
</template>

<script>
import City from './WeatherPwaCity'

let initialWeatherForecast = {
  key: '2459115',
  label: 'New York, NY',
  created: '2016-07-22T01:00:00Z',
  channel: {
    astronomy: {
      sunrise: '5:43 am',
      sunset: '8:21 pm'
    },
    item: {
      condition: {
        text: 'Windy',
        date: 'Thu, 21 Jul 2016 09:00 PM EDT',
        temp: 56,
        code: 24
      },
      forecast: [
        {code: 44, high: 86, low: 70},
        {code: 44, high: 94, low: 73},
        {code: 4, high: 95, low: 78},
        {code: 24, high: 75, low: 89},
        {code: 24, high: 89, low: 77},
        {code: 44, high: 92, low: 79},
        {code: 44, high: 89, low: 77}
      ]
    },
    atmosphere: {
      humidity: 56
    },
    wind: {
      speed: 25,
      direction: 195
    }
  }
}

export default {
  name: 'weather-pwa-main',
  data () {
    return {
      cities: [
        initialWeatherForecast
      ]
    }
  },
  created () {
    this.$parent.bus.$on('forecast-refresh', () => {
      for (let city of this.cities) {
        this.getForecast(city)
      }
    })

    this.$parent.bus.$on('add-city', (city) => {
      this.getForecast(city).then((city) => {
        this.cities.push(city)
      })
    })
  },
  methods: {
    getForecast (city) {
      let statement = `select * from weather.forecast where woeid=${city.key}`
      let url = `https://query.yahooapis.com/v1/public/yql?format=json&q=${statement}`
      let promise = new Promise((resolve, reject) => {
        this.$http.get(url).then(response => {
          city['channel'] = response.data.query.results.channel
          resolve(city)
        })
      })

      return promise
    }
  },
  components: {
    City
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.main {
  height: 100%;
  padding-top: 60px;
  -webkit-box-flex: 1;
  -webkit-flex: 1;
      -ms-flex: 1;
          flex: 1;
  overflow-x: hidden;
  overflow-y: auto;
  -webkit-overflow-scrolling: touch;
}

.card {
  padding: 16px;
  position: relative;
  box-sizing: border-box;
  background: #fff;
  border-radius: 2px;
  margin: 16px;
  box-shadow: 0 2px 2px 0 rgba(0, 0, 0, 0.14), 0 3px 1px -2px rgba(0, 0, 0, 0.2), 0 1px 5px 0 rgba(0, 0, 0, 0.12);
}
</style>
