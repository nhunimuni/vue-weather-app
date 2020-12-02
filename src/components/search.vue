<template>
  <div class="search-box">
    <div class="md-layout-item">
      <md-field>
        <md-icon>search</md-icon>
        <label>Search ...</label>
        <md-input v-model="query" @keypress="changeQueryInput()"></md-input>
      </md-field>
    </div>
  </div>
</template>

<script>
export default {
  name: "Search",
  props: {
    queryInput: String,
  },
  data() {
    return {
      query: "Berlin",
      weather: {},
    };
  },
  methods: {
    changeQueryInput() {
      if (event.key === "Enter") {
        this.$emit("changeQueryInput", this.query);
      }
    },
  },
};
</script>
