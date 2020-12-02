<template>
  <div id="app">
    <main>
      <search @changeQueryInput="query = $event"></search>
      <display
        v-if="typeof weather.main !== 'undefined'"
        :weather="weather"
      ></display>
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
import Display from "./components/display.vue";
import Search from "./components/search.vue";

export default {
  name: "App",
  components: {
    Display,
    Search,
  },
  data() {
    return {
      url_base: "http://api.openweathermap.org/data/2.5/",
      query: "Berlin",
      weather: {},
      api_key: API_KEY.key,
    };
  },
  beforeMount() {
    // `this` points to the vm instance
    fetch(
      `${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`
    )
      .then((res) => {
        return res.json();
      })

      .then(this.setResults);
  },
  watch: {
    query: function(value) {
      if (value) this.fetchWeather();
    },
  },
  methods: {
    fetchWeather() {
      fetch(
        `${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`
      )
        .then((res) => {
          return res.json();
        })
        .then((data) => {
          this.setResults(data);
        })
        .catch((err) => {
          console.log(err);
        });
    },
    setResults(results) {
      this.weather = results;
    },
  },
};
</script>
