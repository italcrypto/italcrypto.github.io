

<template>
  <div>
    <h1 class="text-primary text-center">{{msg}}</h1>



    <button class="btn btn-primary" v-on:click="counter += 1">{{ counter }}</button>
  
  
  
  
<table class="table table-hover">
  <thead>
    <tr>
      <td>Rank</td>
      <td>Name</td>
      <td>Symbol</td>
      <td>Price (USD)</td>
      <td>1H</td>
      <td>1D</td>
      <td>1W</td>
      <td>Market Cap (USD)</td>
      </tr>
  </thead>
<tbody>

    <tr v-for="coin in coins">
      <td>{{ coin.rank }}</td>
      <td>{{ coin.name }}</td>
      <td>{{ coin.symbol }}</td>
      <td>{{ coin.price_usd | currency }}</td>
      <td v-bind:style="getColor(coin.percent_change_1h)">
        <span v-if="coin.percent_change_1h > 0">+</span>{{ coin.percent_change_1h }}%
      </td>
      <td v-bind:style="getColor(coin.percent_change_24h)">
        <span v-if="coin.percent_change_24h > 0">+</span>{{ coin.percent_change_24h }}%
      </td>
      <td v-bind:style="getColor(coin.percent_change_7d)">
        <span v-if="coin.percent_change_7d > 0">+</span>{{ coin.percent_change_7d }}%
      </td>
      <td>{{ coin.market_cap_usd | currency }}</td>
    </tr>
  </tbody>

</table>
  
  
  </div>





</template>

<script>

import axios from 'axios';


let CRYPTOCOMPARE_API_URI = "https://www.cryptocompare.com";

// The API we're using for grabbing cryptocurrency prices.  The service can be
// found at: https://coinmarketcap.com/api/
let COINMARKETCAP_API_URI = "https://api.coinmarketcap.com";

// The amount of milliseconds (ms) after which we should update our currency
// charts.
let UPDATE_INTERVAL = 60 * 1000;

setInterval(() => {
  Home.getCoins();
}, UPDATE_INTERVAL);

export default {
  name: 'Home',
  data () {
    return {
      msg: 'Benvenuti su ItaCrypto News',
      counter: 10,
      coins: [],
       coinData: {}

    }
  },
  methods: {

    /**
     * Load up all cryptocurrency data.  This data is used to find what logos
     * each currency has, so we can display things in a friendly way.
     */
   getCoinData: function() {
  let self = this;

  axios.get(CRYPTOCOMPARE_API_URI + "/api/data/coinlist")
    .then((resp) => {
      this.coinData = resp.data.Data;
      this.getCoins();
    })
    .catch((err) => {
      this.getCoins();
      console.error(err);
    });
},
getColor: (num) => {
  return num > 0 ? "color:green;" : "color:red;";
},

    /**
     * Get the top 10 cryptocurrencies by value.  This data is refreshed each 5
     * minutes by the backing API service.
     */
    getCoins: function() {

       let self = this;

  axios.get(COINMARKETCAP_API_URI + "/v1/ticker/?limit=10")
    .then((resp) => {
      this.coins = resp.data;
    })
    .catch((err) => {
      console.error(err);
    });

    },

    /**
     * Given a cryptocurrency ticket symbol, return the currency's logo
     * image.
     */
  },
  created: function () {
    // `this` points to the vm instance
    this.getCoinData();
    alert("ok");
    console.log('a is: ' + this.a)
  }
 





}
</script>

<style scoped>
h1 {color: #000}
</style>