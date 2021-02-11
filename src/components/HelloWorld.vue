<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <ul>
      <li v-for="(rate, key) in rates" :key="key">
        {{ `${key} = ${rate}` }}
      </li>
    </ul>

    <p>Base Currency</p>
    <select v-model="baseCurrency">
      <option v-for="(rate, key) in rates" :key="key" :value="key">{{ key }}</option>
    </select>

    <p>Target Currency</p>
    <select v-model="targetCurrency">
      <option v-for="(rate, key) in rates" :key="key" :value="key">{{ key }}</option>
    </select>

    <p>Input Amount</p>
    <input type="text" v-model="amount">
    <p>Result</p>
    <strong>{{ targetCurrency }} {{ convertedAmount }}</strong>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'HelloWorld',
  props: {
    msg: String,
  },
  data: () => ({
    rates: [],
    amount: 1,
    targetCurrency: null,
    baseCurrency: null,
  }),
  methods: {
    async fetchData(currency = 'USD') {
      const url = `https://api.openrates.io/latest?base=${currency}`;
      // destruct the object
      const {
        data: {
          rates,
        },
      } = await axios.get(url);
      this.rates = rates;
    },
  },
  watch: {
    baseCurrency(val) {
      this.fetchData(val);
    },
  },
  computed: {
    convertedAmount() {
      // ternary operation
      return this.targetCurrency ? this.rates[this.targetCurrency] * this.amount : 0;
    },
  },
  mounted() {
    this.fetchData();
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
