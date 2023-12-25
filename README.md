# Overview

This project aims to convert [SimpleChart.js](https://github.com/Mathieu2301/TradingView-API/blob/main/examples/SimpleChart.js) example from [Mathieu2301/TradingView-API](https://github.com/Mathieu2301/TradingView-API) into a Vue.js 3 application that lets you.

1. Select market, timeframe and chart type
2. Listen to price changes on the market according to settings

### Current Problem

TradingViewAPI is not meant to run on a browser so the websocket code is not working. I'm considering two solutions

1. Use Server Side Rendering (same repo) to run websocket code from the server
2. Split the repo in two (vue.js app + node.js server) and make a websocket connection between both

# How to run app

```
$ npm i
$ npm run dev
```

Will be running on localhost:5173

# How to run TradingViewAPI/example/SimpleChart.js

```
$ node src/tradingview.js
```

Then check logs

# Vue 3 + Vite

This template should help get you started developing with Vue 3 in Vite. The template uses Vue 3 `<script setup>` SFCs, check out the [script setup docs](https://v3.vuejs.org/api/sfc-script-setup.html#sfc-script-setup) to learn more.

## Recommended IDE Setup

- [VS Code](https://code.visualstudio.com/) + [Volar](https://marketplace.visualstudio.com/items?itemName=Vue.volar) (and disable Vetur) + [TypeScript Vue Plugin (Volar)](https://marketplace.visualstudio.com/items?itemName=Vue.vscode-typescript-vue-plugin).
