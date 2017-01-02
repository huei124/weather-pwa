<template>
  <div style="height: 100%;">
    <weather-pwa-header></weather-pwa-header>
    <weather-pwa-main></weather-pwa-main>
    <weather-pwa-add-city-dialog v-if="showDialog"></weather-pwa-add-city-dialog>
  </div>
</template>

<script>
import WeatherPwaHeader from './components/WeatherPwaHeader'
import WeatherPwaMain from './components/WeatherPwaMain'
import WeatherPwaAddCityDialog from './components/WeatherPwaAddCityDialog'

import Vue from 'vue'
import VueResource from 'vue-resource'

Vue.use(VueResource)

let bus = new Vue()

export default {
  name: 'app',
  data () {
    return {
      bus: bus,
      showDialog: false
    }
  },
  created () {
    this.bus.$on('show-dialog', (visible) => {
      this.showDialog = visible
    })
  },
  components: {
    WeatherPwaHeader,
    WeatherPwaMain,
    WeatherPwaAddCityDialog
  }
}
</script>
