<template>
  <section>
    <div class="container">
      <div class="row">
        <div class="col-sm-12 col-md-12 col-lg-12">
          <div class="card custom-primary">
            <div class="search-bar">
              <h1 class="custom-primary-text">Weather App</h1>
              <div class="mx-4 mt-2">
                <input class="w-full" placeholder="search location" v-model="searchvalue" @input="onSearchValueChange" />
              </div>
              <div class="mx-4">
                <ol class="list-none p-0">
                  <button class="border-0 w-full text-left"
                    v-if="searchvalue.length && !searchResult.length">Loading...</button>
                  <li v-for="(item, index) in searchResult" :key="index">
                    <button class="border-0 w-full text-left" data-bs-toggle="offcanvas" href="#offcanvasExample"
                      role="button" aria-controls="offcanvasExample" @click="fetchWeather(item.name)"
                      v-if="searchResult.length">
                      {{ item.name }} - {{ item.country }}</button>
                  </li>
                </ol>
              </div>
            </div>
          </div>
          <div>
          </div>
        </div>
      </div>
    </div>
  </section>
  <div>
    <div class="offcanvas offcanvas-end custom-offcanvas" tabindex="-1" id="offcanvasExample"
      aria-labelledby="offcanvasExampleLabel">
      <div class="offcanvas-header">
        <h5 class="offcanvas-title" id="offcanvasExampleLabel">{{ selectedLocation }}</h5>
        <button type="button" class="btn-close" data-bs-dismiss="offcanvas" aria-label="Close"></button>
      </div>
      <div class="offcanvas-body">
        <WeatherView msg="Welcome to Your Vue.js App" :dataObject="weatherData" />
      </div>
    </div>
  </div>
</template>

<script>
import WeatherView from './components/WeatherView.vue';

export default {
  name: 'App',
  components: {
    WeatherView
  },
  data() {
    return {
      searchvalue: '',
      baseUrl: 'https://api.weatherapi.com/v1/',
      apiKey: 'ab851d0560a442db8b995631230211',
      searchResult: [],
      weatherData: {},
      selectedLocation: '',
      debounceTimer: null,
    }
  },
  methods: {
    async fetchWeather(location) {
      this.selectedLocation = location;
      fetch(`${this.baseUrl}current.json?q=${location}&key=${this.apiKey}`)
        .then((res) => res.json())
        .then((data) => this.weatherData = data)
        // .then((data) => console.log(data, "DDD"))
        .catch((error) => console.log(error, "ERROR"))

    },

    async fetchApiData(value) {
      fetch(`${this.baseUrl}search.json?q=${value}&key=${this.apiKey}`)
        .then((res) => res.json())
        .then((data) => this.searchResult = data)
        // .then((data) => console.log(data, "DDD"))
        .catch((error) => console.log(error, "ERROR"))
    },
    async fetchData() {
      try {
        this.apiData = await this.fetchApiData(this.searchvalue);
      } catch (error) {
        console.error('API call failed:', error);
      }
    },
    onSearchValueChange() {
      clearTimeout(this.debounceTimer);

      this.debounceTimer = setTimeout(() => {
        this.fetchData();
      }, 1000);
    },
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.search-bar button {
  text-align: left;
}

.search-bar button:hover {
  background: lightblue;
}

.custom-offcanvas {
  width: 75% !important;
  background: azure !important;
}

@media screen and (max-width: 720px) {
  .custom-offcanvas {
    width: 100% !important;
  }
}
</style>
