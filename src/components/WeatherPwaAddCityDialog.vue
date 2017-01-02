<template>
  <div class="dialog-container dialog-container--visible">
    <div class="dialog">
      <div class="dialog-title">Add new city</div>
      <div class="dialog-body">
        <select id="selectCityToAdd" v-model="selectedCity">
          <!-- Values map to Yahoo Weather API Where On Earth Identifiers (WOEIDs).
               https://developer.yahoo.com/weather/documentation.html#req -->
          <option v-for="city in cities" :value="city">{{ city.label }}</option>
        </select>
      </div>
      <div class="dialog-buttons">
        <button @click="add" id="butAddCity" class="button">Add</button>
        <button @click="cancel" id="butAddCancel" class="button">Cancel</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'weather-pwa-add-city-dialog',
  data () {
    return {
      cities: [
        {
          key: '2357536',
          label: 'Austin, TX'
        },
        {
          key: '2367105',
          label: 'Bosten, MA'
        },
        {
          key: '2379574',
          label: 'Chicago, IL'
        },
        {
          key: '2459115',
          label: 'New York, NY'
        },
        {
          key: '2475687',
          label: 'Portland, OR'
        },
        {
          key: '2487956',
          label: 'San Francisco, CA'
        },
        {
          key: '2490383',
          label: 'Seattle, WA'
        }
      ],
      selectedCity: {key: '2357536', label: 'Austin, TX'}
    }
  },
  methods: {
    add () {
      this.$parent.bus.$emit('add-city', this.selectedCity)
      this.$parent.bus.$emit('show-dialog', false)
    },
    cancel () {
      this.$parent.bus.$emit('show-dialog', false)
    }
  }
}
</script>

<style scoped>
.dialog-container {
  background: rgba(0, 0, 0, 0.57);
  position: fixed;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  opacity: 0;
  pointer-events: none;
  will-change: opacity;
  -webkit-transition: opacity 0.333s cubic-bezier(0, 0, 0.21, 1);
  transition: opacity 0.333s cubic-bezier(0, 0, 0.21, 1);
}

.dialog-container--visible {
  opacity: 1;
  pointer-events: auto;
}

.dialog {
  background: #FFF;
  border-radius: 2px;
  box-shadow: 0 0 14px rgba(0, 0, 0, 0.24), 0 14px 28px rgba(0, 0, 0, 0.48);
  min-width: 280px;
  position: absolute;
  left: 50%;
  top: 50%;
  -webkit-transform: translate(-50%, -50%) translateY(30px);
          transform: translate(-50%, -50%) translateY(30px);
  -webkit-transition: -webkit-transform 0.333s cubic-bezier(0, 0, 0.21, 1) 0.05s;
  transition: -webkit-transform 0.333s cubic-bezier(0, 0, 0.21, 1) 0.05s;
  transition: transform 0.333s cubic-bezier(0, 0, 0.21, 1) 0.05s;
  transition: transform 0.333s cubic-bezier(0, 0, 0.21, 1) 0.05s, -webkit-transform 0.333s cubic-bezier(0, 0, 0.21, 1) 0.05s;
}

.dialog > div {
  padding-left: 24px;
  padding-right: 24px;
}

.dialog-title {
  padding-top: 20px;
  font-size: 1.25em;
}

.dialog-body {
  padding-top: 20px;
  padding-bottom: 24px;
}

.dialog-body select {
  width: 100%;
  font-size: 2em;
}

.dialog-buttons {
  padding: 8px !important;
  float: right;
}
</style>
