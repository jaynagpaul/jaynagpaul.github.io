---
layout: post
title: Getting Started With Algorithmic Crypto Trading
description: This tutorial will detail over how to setup a cryptocurrency trading bot to automatically trade coins in order to gain a profit.
image: assets/images/zenbot.png
tags: cryptocurrency bot tutorial
comments: false
---
## Disclaimer
In no way am I a financial advisor or an expert in this field. I would only recommend trying out with small amounts you are willing to lose for educational purposes. Along with this, much of crypto is still a wild wild west, which no person or bot can predict.

## Setup
For our trading bot we'll be using Zenbot which describes itself as a "command-line cryptocurrency trading bot using Node.js and MongoDB". You can view the project's [Github here](https://github.com/DeviaVir/zenbot).
This guide assumes a fresh installation of Ubuntu 16.04, but many of these commands can be easily modified to work on other OS's.

### Requirements
- NodeJS

```shell
# This installs the latest version of NodeJS 8 (8.x)
$ curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
$ sudo apt-get install -y nodejs
```
- MongoDB

```shell
# Import the public key used by the package management system.
$ sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 2930ADAE8CAF5059EE73BB4B58712A2291FA4AD5

# Create a list file for Mongo
$ echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/3.6 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.6.list

# Update the package list
$ sudo apt-get update

# Install the latest version of Mongo
$ sudo apt-get install -y mongodb-org

# Start Mongo
$ sudo service mongod start

# To Stop: sudo service mongod stop
# To Restart: sudo service mongod restart
```

### Installing Zenbot
Now we'll clone, and setup the latest version of Zenbot as per it's [Github Repo](https://github.com/DeviaVir/zenbot).
```shell
# Clone the repo
$ git clone https://github.com/DeviaVir/zenbot

# Enter in the folder and copy over the configuration file if needed
$ cd zenbot
$ cp conf-sample.js conf.js
```
Now you'll have to edit the config to fit your use case. You can use nano by running `nano conf.js` or you can use any other tool. Possible customization options:
- Notifications
    - Pushbullet
    - Slack
    - IFTT
    - XMPP
    - Discord
    - Prowl
    - TextBelt
- Exchanges
    - GDAX
    - Poloniex
    - Kraken
    - Bittrex
    - Quadriga
    - Gemini
    - Bitfinex
    - CEX.IO
    - Bitstamp
- Currency Pairs (`$ zenbot list-selectors`)

- Trading Strategies (`$ zenbot list-strategies`)
    - bollinger -
        Buy when (Signal ≤ Lower Bollinger Band) and sell when (Signal ≥ Upper Bollinger Band).
    - cci_srsi -
        Stochastic CCI Strategy
    - crossover_vwap -
        Estimate trends by comparing "Volume Weighted Average Price" to the "Exponential Moving Average".
    - dema -
        Buy when (short ema > long ema) and sell when (short ema < long ema).
    - forex_analytics -
        Apply the trained forex analytics model.
    - macd -
        Buy when (MACD - Signal > 0) and sell when (MACD - Signal < 0).
    - neural -
        Use neural learning to predict future price. Buy = mean(last 3 real prices) < mean(current & last prediction)
    - noop -
        Just do nothing. Can be used to e.g. generate candlesticks for training the genetic forex strategy.
    - rsi -
        Attempts to buy low and sell high by tracking RSI high-water readings.
    - sar -
        Parabolic SAR
    - speed -
        Trade when % change from last two 1m periods is higher than average.
    - srsi_macd -
        Stochastic MACD Strategy
    - stddev -
        Buy when standard deviation and mean increase, sell on mean decrease.
    - ta_ema -
        Buy when (EMA - last(EMA) > 0) and sell when (EMA - last(EMA) < 0). Optional buy on low RSI.
    - ta_macd -
        Buy when (MACD - Signal > 0) and sell when (MACD - Signal < 0).
    - trend_ema (default) -
        Buy when (EMA - last(EMA) > 0) and sell when (EMA - last(EMA) < 0). Optional buy on low RSI.
    - trendline -
        Calculate a trendline and trade when trend is positive vs negative.
    - trust_distrust -
        Sell when price higher than $sell_min% and highest point - $sell_threshold% is reached. Buy when lowest price point + $buy_threshold% reached.


## Running Zenbot with paper trading.
```shell 
# Install NPM dependencies
$ npm install

# Start paper trading
$ ./zenbot.sh trade --paper
# If customized:
$ ./zenbot.sh trade --paper --conf ./conf.js
```
It will now download previous data in order to train the strategy set for the bot. This may take awhile.
![Fetching Previous Data](assets/images/preroll.png)

When it's done you'll see something like this:
![Zenbot in Action](assets/images/zenbot-in-action.png)
This chart shows the latest prices of the selector we chose (ex. BTC-USD), along with recent actions the bot has taken.

## Manual Trading
Awesome! We now have a fully functional simulated crypto currency trading bot. If needed you can override the bot and manually set limit buy or sell orders by pressing the keys `B` or `S` respectively. You can cancel these orders if you change your mind by pressing `C`.

## Foreword
If you're looking for more advanced usage, check out the [repository](https://github.com/DeviaVir/zenbot). If you're looking to trade with real money, proceed with utter caution and don't forget about securing your server, database, and exchange account. I hope you found this article helpful and it helped you in your hodling endeavors.