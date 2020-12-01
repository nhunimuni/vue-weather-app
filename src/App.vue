<template>
  <div id="app">
    <main>
      <div class="search-box">
        <div class="md-layout-item">
          <md-field>
            <md-icon>search</md-icon>
            <label>Search ...</label>
            <md-input v-model="query" @keypress="fetchWeather"></md-input>
          </md-field>
        </div>
      </div>

      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="weather__date md-subheading">{{ dateBuilder() }}</div>
        <div class="weather__location md-title">
          {{ weather.name }}, {{ weather.sys.country }}
          <div class="weather__location__state">
            <md-icon v-if="weather.weather[0].main === 'Clouds'">cloud</md-icon>
            <div class="weather__results__weather md-body-1">
              {{ weather.weather[0].main }}
            </div>
          </div>
        </div>
        <md-content class="weather__results__temp md-primary md-body-1">
          <div class="temp md-headline">
            {{ Math.round(weather.main.temp) }}°C
          </div>
          <div class="temp__additional">
            <div class="temp-max md-caption">
              H: {{ Math.round(weather.main.temp_max) }}°C
            </div>
            <div class="temp-low md-caption">
              L: {{ Math.round(weather.main.temp_min) }}°C
            </div>
          </div>
        </md-content>

        <div class="weather__results__humidity md-body-1">
          Humidity: {{ weather.main.humidity }}%
        </div>

        <div class="weather__results__wind md-body-1">
          Wind: {{ weather.wind.speed }}km/h
        </div>
      </div>
      <md-empty-state
        v-else
        md-icon="sentiment_dissatisfied"
        md-label="Ops something went wrong. "
        md-description="City or country could not be found."
      >
      </md-empty-state>
    </main>
  </div>
</template>

<script>
import API_KEY from "../openweathermmap-key";

export default {
  name: "App",
  data() {
    return {
      url_base: "http://api.openweathermap.org/data/2.5/",
      query: "Berlin",
      weather: {},
    };
  },
  beforeMount() {
    // `this` points to the vm instance
    fetch(
      `${this.url_base}weather?q=${this.query}&units=metric&APPID=${API_KEY.key}`
    )
      .then((res) => {
        return res.json();
      })
      .then(this.setResults);
  },
  methods: {
    fetchWeather(event) {
      if (event.key === "Enter") {
        fetch(
          `${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`
        )
          .then((res) => {
            console.log(res);
            return res.json();
          })
          .then(this.setResults)
          .catch((err) => {
            console.log(err);
          });
      }
    },
    setResults(results) {
      this.weather = results;
    },
    dateBuilder() {
      let d = new Date();
      let months = [
        "January",
        "February",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "October",
        "November",
        "December",
      ];
      let days = [
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday",
        "Sunday",
      ];

      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day}, ${date} ${month} ${year}`;
    },
  },
};
</script>
