[日本語](public-api_JP.md)

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Public REST API for Bitbank (2023-04-27)](#public-rest-api-for-bitbank-2023-04-27)
  - [General API Information](#general-api-information)
  - [General endpoints](#general-endpoints)
    - [Ticker](#ticker)
    - [Tickers](#tickers)
    - [TickersJPY](#tickersjpy)
    - [Depth](#depth)
    - [Transactions](#transactions)
    - [Candlestick](#candlestick)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Public REST API for Bitbank (2023-04-27)

## General API Information

- The base endpoint is: **[https://public.bitbank.cc](https://public.bitbank.cc)**
- HTTP `4XX` return codes are used for malformed requests; the issue is on the sender's side.
- Any endpoint can return an ERROR; the error payload is as follows:

```json
{
  "success": 0,
  "data": {
    "code": 10000
  }
}
```

## General endpoints

### Ticker

Get Ticker information

```txt
GET /{pair}/ticker
```

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
pair | string | YES | pair enum: [pair list](pairs.md)

**Response:**

Name | Type | Description
------------ | ------------ | ------------
sell | string | the lowest price of sell orders
buy | string | the highest price of buy orders
high | string | the highest price in last 24 hours
low | string | the lowest price in last 24 hours
open | string | the open price at 24 hours ago
last | string | the latest price executed
vol | string | trading volume in last 24 hours
timestamp | number | ticked at unix timestamp (milliseconds)

response format:

```json
{
  "success": 1,
  "data": {
    "sell": "string",
    "buy": "string",
    "high": "string",
    "low": "string",
    "open": "string",
    "last": "string",
    "vol": "string",
    "timestamp": 0
  }
}
```

### Tickers

Get All Tickers information

```txt
GET /tickers
```

**Parameters:**

nothing

**Response:**

Name | Type | Description
------------ | ------------ | ------------
pair | string | pair enum: [pair list](pairs.md)
sell | string | the lowest price of sell orders
buy | string | the highest price of buy orders
high | string | the highest price in last 24 hours
low | string | the lowest price in last 24 hours
open | string | the open price at 24 hours ago
last | string | the latest price executed
vol | string | trading volume in last 24 hours
timestamp | number | ticked at unix timestamp (milliseconds)

response format:

```json
{
  "success": 1,
  "data": [{
    "pair": "string",
    "sell": "string",
    "buy": "string",
    "high": "string",
    "low": "string",
    "open": "string",
    "last": "string",
    "vol": "string",
    "timestamp": 0
  }]
}
```

### TickersJPY

Get All JPY Pair Tickers information

```txt
GET /tickers_jpy
```

**Parameters:**

nothing

**Response:**

Name | Type | Description
------------ | ------------ | ------------
pair | string | JPY pair enum: [pair list](pairs.md)
sell | string | the lowest price of sell orders
buy | string | the highest price of buy orders
high | string | the highest price in last 24 hours
low | string | the lowest price in last 24 hours
open | string | the open price at 24 hours ago
last | string | the latest price executed
vol | string | trading volume in last 24 hours
timestamp | number | ticked at unix timestamp (milliseconds)

response format:

```json
{
  "success": 1,
  "data": [{
    "pair": "string",
    "sell": "string",
    "buy": "string",
    "high": "string",
    "low": "string",
    "open": "string",
    "last": "string",
    "vol": "string",
    "timestamp": 0
  }]
}
```

### Depth

Get Depth information.

```txt
GET /{pair}/depth
```

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
pair | string | YES | pair enum: [pair list](pairs.md)

**Response:**

Name | Type | Description
------------ | ------------ | ------------
asks | [string, string][] | array of [price, amount]
bids | [string, string][] | array of [price, amount]

response format:

```json
{
  "success": 1,
  "data": {
    "asks": [
      [
        "string",  "string"
      ]
    ],
    "bids": [
      [
        "string",  "string"
      ]
    ]
  }
}
```

### Transactions

Get latest executed transactions. If you omit YYYYMMDD, you can get the latest 60.

```txt
GET /{pair}/transactions/{YYYYMMDD}
```

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
pair | string | YES | pair enum: [pair list](pairs.md)
YYYYMMDD | string | NO | date formatted as `YYYYMMDD`

**Response:**

Name | Type | Description
------------ | ------------ | ------------
transaction_id | string | transaction id
side | string | enum: `buy`, `sell`
price | string | price
amount | string |amount
executed_at | number | executed at unix timestamp (milliseconds)

response format:

```json
{
  "success": 1,
  "data": {
    "transactions": [
      {
        "transaction_id": 0,
        "side": "string",
        "price": "string",
        "amount": "string",
        "executed_at": 0
      }
    ]
  }
}
```

### Candlestick

Get candlestick information.

```txt
GET /{pair}/candlestick/{candle-type}/{YYYY}
```

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
pair | string | YES | pair enum: [pair list](pairs.md)
candle-type | string | YES | candle type enum: `1min`, `5min`, `15min`, `30min`, `1hour`, `4hour`, `8hour`, `12hour`, `1day`, `1week`, `1month`
YYYY | string | YES | date formatted as `YYYY` or  `YYYYMMDD`

- YYYY Format depends on the candle-type:
  - `YYYYMMDD`: `1min`, `5min`, `15min`, `30min`, `1hour`
  - `YYYY`: `4hour`, `8hour`, `12hour`, `1day`, `1week`, `1month`

**Response:**

Name | Type | Description
------------ | ------------ | ------------
type | string | candle type enum: `1min`, `5min`, `15min`, `30min`, `1hour`, `4hour`, `8hour`, `12hour`, `1day`, `1week`, `1month`
ohlcv | [string, string, string, string, string, number][] | [open, high, low, close, volume, **unix timestamp (milliseconds)**]

response format:

```json
{
  "success": 1,
  "data": {
    "candlestick": [
      {
        "type": "string",
        "ohlcv": [
          [
            "string",
            "string",
            "string",
            "string",
            "string",
            0
          ]
        ]
      }
    ]
  }
}
```
