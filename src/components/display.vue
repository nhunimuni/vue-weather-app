<template>
  <div class="weather-wrap">
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
      <div class="temp md-headline">{{ Math.round(weather.main.temp) }}°C</div>
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
</template>

<script>
export default {
  name: "Display",
  props: {
    weather: {},
  },
  methods: {
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
