<template>
  <div v-if="city.channel">
    <div class="city-key" hidden></div>
    <div class="card-last-updated" hidden></div>
    <div class="location">{{ city.label }}</div>
    <div class="date">{{ current.date }}</div>
    <div class="description">{{ current.text }}</div>
    <div class="current">
      <div class="visual">
        <div class="icon" :class="getIconClass(current.code)"></div>
        <div class="temperature">
          <span class="value"></span>{{ current.temp }}<span class="scale">°F</span>
        </div>
      </div>
      <div class="description">
        <div class="humidity">{{ humidity }}%</div>
        <div class="wind">
          <span class="value">{{ wind.speed }}</span>
          <span class="scale">mph</span>
          <span class="direction">{{ wind.direction }}</span>°
        </div>
        <div class="sunrise">{{ sunrise }}</div>
        <div class="sunset">{{ sunset }}</div>
      </div>
    </div>
    <div class="future">
      <div v-for="n in 7" class="oneday">
        <div class="date">{{ daysOfWeek(n) }}</div>
        <div class="icon" :class="getIconClass(city.channel.item.forecast[n - 1].code)"></div>
        <div class="temp-high">
          <span class="value">{{ city.channel.item.forecast[n - 1].high }}</span>°
        </div>
        <div class="temp-low">
          <span class="value">{{ city.channel.item.forecast[n - 1].low }}</span>°
        </div>
      </div>
    </div>
  </div>
</template>

<script>
let today = new Date()

export default {
  name: 'city',
  computed: {
    current () {
      return this.city.channel.item.condition
    },
    sunrise () {
      return this.city.channel.astronomy.sunrise
    },
    sunset () {
      return this.city.channel.astronomy.sunset
    },
    humidity () {
      return this.city.channel.atmosphere.humidity
    },
    wind () {
      return this.city.channel
    }
  },
  methods: {
    daysOfWeek (n) {
      return ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'][(n + today.getDay() - 1) % 7]
    },
    getIconClass (code) {
      code = Number(code)
      switch (code) {
        case 25: // cold
        case 32: // sunny
        case 33: // fair (night)
        case 34: // fair (day)
        case 36: // hot
        case 3200: // not available
          return { 'clear-day': true }
        case 0: // tornado
        case 1: // tropical storm
        case 2: // hurricane
        case 6: // mixed rain and sleet
        case 8: // freezing drizzle
        case 9: // drizzle
        case 10: // freezing rain
        case 11: // showers
        case 12: // showers
        case 17: // hail
        case 35: // mixed rain and hail
        case 40: // scattered showers
          return { 'rain': true }
        case 3: // severe thunderstorms
        case 4: // thunderstorms
        case 37: // isolated thunderstorms
        case 38: // scattered thunderstorms
        case 39: // scattered thunderstorms (not a typo)
        case 45: // thundershowers
        case 47: // isolated thundershowers
          return { 'thunderstorms': true }
        case 5: // mixed rain and snow
        case 7: // mixed snow and sleet
        case 13: // snow flurries
        case 14: // light snow showers
        case 16: // snow
        case 18: // sleet
        case 41: // heavy snow
        case 42: // scattered snow showers
        case 43: // heavy snow
        case 46: // snow showers
          return { 'snow': true }
        case 15: // blowing snow
        case 19: // dust
        case 20: // foggy
        case 21: // haze
        case 22: // smoky
          return { 'fog': true }
        case 24: // windy
        case 23: // blustery
          return { 'windy': true }
        case 26: // cloudy
        case 27: // mostly cloudy (night)
        case 28: // mostly cloudy (day)
        case 31: // clear (night)
          return { 'cloudy': true }
        case 29: // partly cloudy (night)
        case 30: // partly cloudy (day)
        case 44: // partly cloudy
          return { 'partly-cloudy-day': true }
      }
    }
  },
  props: ['city']
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.card {
  padding: 16px;
  position: relative;
  box-sizing: border-box;
  background: #fff;
  border-radius: 2px;
  margin: 16px;
  box-shadow: 0 2px 2px 0 rgba(0, 0, 0, 0.14), 0 3px 1px -2px rgba(0, 0, 0, 0.2), 0 1px 5px 0 rgba(0, 0, 0, 0.12);
}

.weather-forecast .location {
  font-size: 1.75em;
}

.weather-forecast .date, .weather-forecast .description {
  font-size: 1.25em;
}

.weather-forecast .current {
  display: -webkit-box;
  display: -webkit-flex;
  display: -ms-flexbox;
  display: flex;
}

.weather-forecast .current .icon {
  width: 128px;
  height: 128px;
}
.weather-forecast .current .visual {
  display: -webkit-box;
  display: -webkit-flex;
  display: -ms-flexbox;
  display: flex;
  font-size: 4em;
}
.weather-forecast .current .visual .scale {
  font-size: 0.5em;
  vertical-align: super;
}
.weather-forecast .current .visual, .weather-forecast .current .description {
  -webkit-box-flex: 1;
  -webkit-flex-grow: 1;
      -ms-flex-positive: 1;
          flex-grow: 1;
}
.weather-forecast .current .sunset:before {
  content: "Sunset: ";
  color: #888;
}
.weather-forecast .current .wind:before {
  content: "Wind: ";
  color: #888;
}
.weather-forecast .current .sunrise:before {
  content: "Sunrise: ";
  color: #888;
}
.weather-forecast .current .humidity:before {
  content: "Humidity: ";
  color: #888;
}
.weather-forecast .current .pollen:before {
  content: "Pollen Count: ";
  color: #888;
}
.weather-forecast .current .pcount:before {
  content: "Pollen ";
  color: #888;
}

.weather-forecast .future {
  display: -webkit-box;
  display: -webkit-flex;
  display: -ms-flexbox;
  display: flex; }
  .weather-forecast .future .oneday {
    -webkit-box-flex: 1;
    -webkit-flex-grow: 1;
        -ms-flex-positive: 1;
            flex-grow: 1;
    text-align: center;
}
.weather-forecast .future .oneday .icon {
  width: 64px;
  height: 64px;
  margin-left: auto;
  margin-right: auto;
}
.weather-forecast .future .oneday .temp-high, .weather-forecast .future .oneday .temp-low {
  display: inline-block;
}
.weather-forecast .future .oneday .temp-low {
  color: #888;
}

.weather-forecast .icon {
  background-repeat: no-repeat;
  background-size: contain;
}

.weather-forecast .icon.clear-day {
  background-image: url("/images/clear.png");
}
.weather-forecast .icon.clear-night {
  background-image: url("/images/clear.png");
}
.weather-forecast .icon.rain {
  background-image: url("/images/rain.png");
}
.weather-forecast .icon.snow {
  background-image: url("/images/snow.png");
}
.weather-forecast .icon.sleet {
  background-image: url("/images/sleet.png");
}
.weather-forecast .icon.windy {
  background-image: url("/images/wind.png");
}
.weather-forecast .icon.fog {
  background-image: url("/images/fog.png");
}
.weather-forecast .icon.cloudy {
  background-image: url("/images/cloudy.png");
}
.weather-forecast .icon.partly-cloudy-day {
  background-image: url("/images/partly-cloudy.png");
}
.weather-forecast .icon.partly-cloudy-night {
  background-image: url("/images/partly-cloudy.png");
}
.weather-forecast .icon.thunderstorms {
  background-image: url("/images/thunderstorm.png");
}

@media (max-width: 450px) {
  .weather-forecast .date, .weather-forecast .description {
    font-size: 0.9em;
  }
  .weather-forecast .current .icon {
    width: 96px;
    height: 96px;
  }
  .weather-forecast .current .visual {
    font-size: 3em;
  }
  .weather-forecast .future .oneday .icon {
    width: 32px;
    height: 32px;
  }
}
</style>
