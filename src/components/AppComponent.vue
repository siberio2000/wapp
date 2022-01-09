<template>
  <div id="wapp">
    <main>
      <div class="title">
        <h4>WEATHER APP</h4>
      </div>

      <div class="search-box">
        <!-- On @keypress we call fetchWeather function -->
        <input
          type="text"
          class="search-bar"
          placeholder="Search town/city"
          v-model="query"
          @keypress="fetchWeather"
        />
      </div>

      <div class="weather-wrap" v-if="(typeof weather.main) != 'undefined'">
        <div class="location-box">
          <div class="location">
            {{ weather.name.toUpperCase() }}, {{ weather.sys.country }}
          </div>
        </div>

        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°c</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
          <div class="info">Source: openweathermap.org</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: "AppComponent",
  data() {
    return {
      api_key: "642b614ab916ba2b01bfaf5b4452bc9c",
      url_base: "https://api.openweathermap.org/data/2.5/",
      query: "",
      weather: [],
    };
  },
  methods: {
    /**
     * Here we are fetching API
     * using documentation from https://openweathermap.org/current
     * also we are handling success and error with simple tests
     */
    fetchWeather(e) {
      if (e.key == "Enter") {
        fetch(
          `${this.url_base}weather?q=${this.query}&units=metric&appid=${this.api_key}`
        )
          .then((res) => {
            if (!res.ok) {
              throw Error(res.statusText);
            }
            return res.json();
          })
          .then(this.setResults)
          .catch((error) => alert(error))
          .finally(() => {
            console.log("API fetched");
          });
      }
    },
    /**
     * Injecting fetched data into weather array
     * And then use it within DOM to display in UI
     */
    setResults(results) {
      this.weather = results;
      console.log(results);
    },
    errorInfo(results) {
      this.weather = results;
    },
  },
};
</script>

<style scoped>
@import "../styles/main.css";
</style>
