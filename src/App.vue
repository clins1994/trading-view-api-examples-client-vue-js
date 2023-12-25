<script setup>
import * as TradingView from '@mathieuc/tradingview';
import { ref } from 'vue';

const client = ref(null); // creates websocket client
const chart = ref(null); // init a chart session
const market = ref('BINANCE:BTCEUR'); // market
const timeframe = ref('15'); // minutes
const chartType = ref('HeikinAshi'); // chart type
const priceChangeLogs = ref([]); // priceChangeLogs

const initClient = () => {
  client.value = new TradingView.Client();
};

const initChart = () => {
  chart.value = new client.Session.Chart();

  // Set the market
  chart.value.setMarket('BINANCE:BTCEUR', { timeframe: 'D' });

  // Listen for errors (can avoid crash)
  chart.onError((...err) => {
    console.error('Chart error:', ...err);
  });

  // When the symbol is successfully loaded
  chart.value.onSymbolLoaded(() => {
    console.log(`Market "${chart.infos.description}" loaded !`);
  });

  // When price changes
  chart.value.onUpdate(() => {
    if (!chart.value.periods[0]) return;
    const priceChangeLog = `[${chart.value.infos.description}]: ${chart.value.periods[0].close} ${chart.value.infos.currency_id}`;
    console.log(priceChangeLog);
    priceChangeLogs.value.push(priceChangeLog);
  });
};

const setTimeframe = () => {
  console.log('\nSetting timeframe to 15 minutes...');
  chart.value.setSeries(timeframe.value);
};

const setMarket = () => {
  console.log('\nSetting market to BINANCE:ETHEUR...');
  chart.value.setMarket(market.value, {
    timeframe: 'D',
  });
};

const setChartType = () => {
  console.log('\nSetting the chart type to "Heikin Ashi"s...');
  chart.value.setMarket(market.value, {
    timeframe: 'D',
    type: chartType.value,
  });
};

const closeChart = () => {
  console.log('\nClosing the chart...');
  chart.value.delete();
  chart.value = null;
};

const closeClient = () => {
  console.log('\nClosing the client...');
  client.value.end();
  client.value = null;
};
</script>

<template>
  <div class="container">
    <div class="left-column">
      <label for="market">Market:</label>
      <input v-model="market" id="market" />

      <label for="timeframe">Timeframe:</label>
      <input v-model="timeframe" id="timeframe" />

      <label for="chartType">Chart Type:</label>
      <input v-model="chartType" id="chartType" />

      <button @click="initClient">Initialize Client</button>
      <button @click="initChart">Initialize Chart</button>
      <button @click="setTimeframe">Set Timeframe</button>
      <button @click="setMarket">Set Market</button>
      <button @click="setChartType">Set Chart Type</button>
      <button @click="closeChart">Close Chart</button>
      <button @click="closeClient">Close Client</button>
    </div>

    <div class="right-column">
      <table>
        <thead>
          <tr>
            <th>Price Change Logs</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(log, index) in priceChangeLogs" :key="index">
            <td>{{ log }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<style scoped>
.container {
  display: flex;
  justify-content: space-between;
  margin: 20px;
}

.left-column {
  width: 200px;
  background-color: lightblue;
  align-items: center;
}

.right-column {
  width: 300px;
  align-items: center;
}

label,
input,
button {
  display: block;
  margin-bottom: 10px;
}

table {
  width: 100%;
}

th,
td {
  border: 1px solid #ddd;
  padding: 8px;
  text-align: left;
}
</style>
