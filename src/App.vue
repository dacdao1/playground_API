<template>
  <div id="app">
    <h1>Bitcoin Price Index</h1>
    <div v-for="currency in info" v-bind:key="currency" class="currency">
      {{ currency.description }}:
      <span class="lighten">
        <span v-html="currency.symbol"></span>
        {{ currency.rate_float | currencydecimal }}
      </span>
    </div>
    <br>
    <div>{{info2}}</div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "App",
  data() {
    return {
      info: null,
      info2: null
    };
  },

  //this is use with just 1 way in and out with restful call
  mounted() {
    axios
      .get("https://api.coindesk.com/v1/bpi/currentprice.json")
      .then(response => (this.info = response.data.bpi));
  },

  //this is use with async/await promises.
  async created() {
    try {
      const response = await axios.get(
        "https://api.coindesk.com/v1/bpi/currentprice.json"
      );
      this.info2 = response.data;
    } catch (e) {
      this.errors.push(e);
    }
  },
  filters: {
    currencydecimal(value) {
      return value.toFixed(2);
    }
  },
  components: {}
};
</script>

<style>
</style>
