<template>

  <div id="app">
    <h1>Bitcoin Price Index</h1>

    <section v-if="errored">
      <p>We're sorry, we're not able to retrieve this information at the moment, please try back later</p>
    </section>

    <section v-else>
      <div v-if="loading">Loading...</div>

      <div
        v-else
        v-for="currency in info"
        class="currency"
      >
        {{ currency.description }}:
        <span class="lighten">
        <span v-html="currency.symbol"></span>{{ currency.rate_float | currencydecimal }}
      </span>
      </div>

    </section>
  </div>

</template>

<script>

import moment from 'moment'
import axios from 'axios'

export default {
  name: "basket",
  data() {
    return {
      info : {},
      count: 0
    }
  },
  methods : {
    increment(){
      this.count++;
    },
    decrement(){
      this.count--;
    }
  },
  mounted(){
    console.log(`The initial count is ${moment().format('yyyyMMDD hh:mm:ss')} ${this.count}.`)

    axios
      .get('https://api.coindesk.com/v2/bpi/currentprice.json')
      .then(response => {
        this.info = response.data.bpi
      })
      .catch(error => {
        console.log(error)
      })
      .finally(() => this.loding = false)




  }

}
</script>

<style scoped>

</style>
