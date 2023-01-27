# Table of Contents * [Fairdesk Public API](#publicapi) * [General Public API Information](#general)

* [REST API Standards](#restapi)
    * [Common Constants](#commconsts)
        * [Order Side](#orderside)
        * [Position Mode](#positionmode)
        * [Position Side](#positionside)
        * [Order Type](#ordertype)
        * [Order Status](#orderstatus)
        * [Time in Force](#timeinforce)
        * [Price Trigger Type](#pricetriggertype)
    * [Public REST API List](#restpublicapilist)
        * [product info](#queryproductinfo)
        * [pair info](#querypair)
        * [ticker](#queryticker)
        * [orderbook](#queryorderbook)
        * [trade history](#querytradehistory)
        * [Kline](#restpublickline)
    * [User REST API List](#restuserapilist)
        * [List Open Orders](#queryopenorders)
        * [Place Order](#placeorder)
        * [Cancel Single Order by Order id](#cancelsingleorder)
        * [Cancel All Orders](#cancelallorders)
        * [Query Trading Account Balance](#querytradeaccount)
        * [Query Trade history](#querytrades)
        * [Query Order History](#orderhistory)
* [Websocket Stream API](#websocket-stream)
    * [Kline/Candlestick Streams](#wskline)
    * [Ticker](#wsticker)
    * [Order Book](#wsorderbook)
    * [Live Trade](#wstrade)
    * [Account Stream](#accountstream)
        * [Order Update](#wsorderupdate)
        * [Account Update](#wsaccountupdate)

<a name="publicapi"/>

# Spot Public API

<a name="general"/>

### Error Codes

[Trading Error Codes](TradeErrorCode.md)
 

<a name="commconsts"/>

### Common Constants

<a name="orderside"/>

#### Order Side

| type | description |
|-----------|-------------|
| BUY       | BUY         |
| SELL      | SELL        |

<a name="ordertype"/>

#### Order Type

| order type         | description                 |
|--------------------|-----------------------------|
| LIMIT              | limit order                 |
| MARKET             | market order                |

<a name="orderstatus"/>

#### Order Status

| order status     | description                                 | 
|------------------|---------------------------------------------|
| NEW              | order accepted                              |
| PARTIALLY_FILLED | order partial filled                        |
| FILLED           | order fully filled                          |
| CANCELED         | order canceled                              |
| EXPIRED          | Order expired                               |

#### Order Exec Type

| execution type | description       | 
|----------------|-------------------|
| E_NEW          | order accepted    |
| E_CANCELED     | order cancelled   |
| E_TRADE        | order traded      |
| E_EXPIRED      | order expired     |

<a name="timeinforce"/>

#### Time in Force

| timeInForce   | description         |
|---------------|---------------------|
| GTC           | good till cancel    |
| POST_ONLY     | post only           |
| IOC           | immediate or cancel |
| FOK           | fill or kill        |

<a name="restpublicapilist"/>

## Public REST API List

<a name="queryproductinfo"/>

#### Query Product Information

* Request：

```
GET /api/v1/public/spot-products 
```

* Example Response:

```json
{
  "status": 0,
  "error": "OK",
  "data": [
    {
      "symbolId": 1211,
      "name": "btcusdt",
      "displayName": "BTCUSDT",
      "baseCcyName": "BTC",
      "quoteCcyName": "USDT",
      "tickSize": 0.100000,
      "stepSize": 0.00100000,
      "maxPrice": 1000000.000000,
      "minPrice": 100.000000,
      "maxOrderQty": 50.00000000,
      "minOrderQty": 0E-8,
      "makerFeeRate": 0.00100000,
      "takerFeeRate": 0.00100000,
      "logoPath": null
    }
  ]
  ...
}

```

* Detail for the Response Field:

| Field           | Type           | Description                       | Possible values      |
|-----------------|----------------|-----------------------------------|----------------------|
| symbolId        | Int            | internal symbol id                |                      |
| symbol          | String         | symbol name                       |                      |
| tickSize        | Decimal        | minimum price incremental size    |                      |
| stepSize        | Decimal        | minimum quantity incremental size |                      |
| maxPrice        | Decimal        | Max price                         |                      |
| minPrice        | Decimal        | min price                         |                      |
| maxOrderQty     | Decimal        | min price                         |                      |
| minOrderQty     | Decimal        | min order size                    |                      |
| makerFeeRate    | Decimal        | default maker fee rate            |                      |
| takerFeeRate    | Decimal        | default taker fee rate            |                      |
  |

<a name="querypair"/>

#### Query Pair info

* Request：

```
GET /api/v1/public/spot/pairs
```

* Example Response:

```json

{
  "success": true,
  "result": [
    {
      "ticker_id": "XRP_USDT",
      "base_currency": "XRP",
      "target_currency": "USDT",
      "last_price": 0.407600,
      "base_volume": 1454497.00000000,
      "bid": 0.406000,
      "ask": 0.407600,
      "high": 0.413300,
      "low": 0.398900
    },
   ...
  ],
  "error": null
}

```



<a name="queryticker"/>

#### Query All Tickers

* Request：

```
GET /api/v1/public/spot/tickers
```

* Example Response:

```json
{
  "success": true,
  "result": [
    {
      "ticker_id": "XRP_USDT",
      "base_currency": "XRP",
      "target_currency": "USDT",
      "last_price": 0.408100,
      "base_volume": 1477301.00000000,
      "bid": 0.406700,
      "ask": 0.408200,
      "high": 0.413300,
      "low": 0.398900
    }
  ],
  "error": null
}

```

<a name="queryorderbook"/>
#### Query Order book

* Request：

```
GET /api/v1/public/spot/orderbook?ticker_id=BTCUSDT&depth=10
```

* Example Response:

```json

{
  "success": true,
  "result": {
    "timestamp": 0,
    "bids": [
      [
        23080.000000,
        0.23700000
      ],
       ...
    ],
    "asks": [
      [
        23087.200000,
        0.90100000
      ],
       ...
    ],
    "ticker_id": "BTCUSDT"
  },
  "error": null
}

```


<a name="querytradehistory"/>
#### Query Trade History

* Request：

```
GET /api/v1/public/spot/historical-trades?ticker_id=BTC_USDT&limit=10&startTime=1674837267000
```

* Example Response:

```json

{
  "success": true,
  "result": [
    {
      "ticker_id": "BTC_USDT",
      "price": 23057.100000,
      "base_volume": 0.93100000,
      "target_volume": 21466.16010000000000,
      "trade_timestamp": 1674833,
      "type": "BUY"
    },
    {
      "ticker_id": "BTC_USDT",
      "price": 23059.400000,
      "base_volume": 1.12700000,
      "target_volume": 25987.94380000000000,
      "trade_timestamp": 1674833,
      "type": "BUY"
    }
    ...
  ],
  "error": null
}

```

<a name="restpublickline"/>

### Kline query

rate limit weight: 10

* Example Request：

```
GET /api/v1/public/spot-md/kline?symbol=BTCUSDT&interval=5m&from=1651382628000&to=1651469028000&limit=100
```

* Request param

| Field    | Type   | Description          | Default values |
|----------|--------|----------------------|----------------|
| symbol   | String | symbol name          |                |
| interval | ENUM   | interval name        | 5m             |
| from     | Long   | start milliseconds   |                |
| to       | Long   | end milliseconds     |                |
| limit    | Int    | date limit, max 1000 | 500            |

* Example Response
*

```json
{
  "status": 0,
  "error": "OK",
  "data": [
    {
      "openTime": 1660146060000,
      "closeTime": 1660146119999,
      "openPrice": "24010.200000",
      "closePrice": "24010.200000",
      "highPrice": "24010.200000",
      "lowPrice": "24010.200000",
      "volume": "0.01200000",
      "quoteVolume": "288.12240000",
      "count": 1,
      "takerBuyVolume": "0.01200000",
      "takerBuyQuoteVolume": "288.12240000"
    },
    {
      "openTime": 1660146000000,
      "closeTime": 1660146059999,
      "openPrice": "24000.600000",
      "closePrice": "24000.600000",
      "highPrice": "24000.600000",
      "lowPrice": "24000.600000",
      "volume": "0.08200000",
      "quoteVolume": "1968.04920000",
      "count": 1,
      "takerBuyVolume": "0.08200000",
      "takerBuyQuoteVolume": "1968.04920000"
    },
    ...
  ]
}
```

* Detail for the Response Field:

| Field       | Type    | Description                   |  
|-------------|---------|-------------------------------|
| openTime    | Long    | interval start time in millis |
| closeTime   | Long    | interval start time in millis |
| interval    | Enum    | kline interval                |           
| open        | Decimal | open price                    |
| high        | Decimal | high price                    |
| low         | Decimal | low price                     |
| close       | Decimal | close price                   |
| closed      | Boolean | is the kline closed           |
| numTrades   | Int     | number of Trades              |
| baseVolume  | Decimal | 24h volume for base ccy       |
| quoteVolume | Decimal | 24h volume for quote ccy      |

<a name="queryopenorders"/>

### List Open Orders

* Request

```
GET /api/v1/private/spot-account/open-orders
```  

* Example Response

```json
{
  "status": 0,
  "error": "OK",
  "data": [
    {
      "orderId": 1523707343,
      "clientOrderId": "WEBjCwL6V6SBQ6e",
      "symbol": "ethusdt",
      "type": "LIMIT",
      "origQty": "0.12",
      "executedQty": "0.00",
      "price": "300.00",
      "timeInForce": "GTC",
      "side": "BUY",
      "transactTime": 1632192208043,
      "status": "NEW"
    },
    ...
  ]
}
```

* Explanation for the Fields

| Field          | Type      | Description                                          | Possible values             |
|----------------|-----------|------------------------------------------------------|-----------------------------|
| symbol         | String    | Which symbol to place order                          |                             | 
| clientOrderId  | String    | client order id, max length is 40                    |                             |
| orderId        | Long      | unique order id                                      |                             |
| status         | Enum      | order execution status                               | NEW,PARTIALLY_FILLED,FILLED |
| side           | Enum      | Order direction, Buy or Sell                         | BUY, SELL                   | 
| transactTime   | Long      | last transaction time                                |
| quantity       | String    | Order quantity                                       |                             |
| price          | String    | price, required for limit order                      |                             | 
| type           | Enum      | order type                                           | refer to orderType          | 
| timeInForce    | Enum      | Time in force.                                       | GTC, FOK, IOC, POST_ONLY    | 
| origQty        | Decimal   | original order quantity                              |                             |
| executedQty    | Decimal   | executed quantity                                    |                             |

<a name="placeorder"/>

### Place Order

* HTTP Request:

```
POST /api/v1/private/spot-trade/place-order

{
  "quantity": "0.01",
  "price": "22601.5",
  "side": "BUY",
  "type": "LIMIT",
  "timeInForce": "GTC",
  "symbol": "btcusdt",
  "clientOrderId": "WEB97h8absNss976",
  "orderRespType": "ACK"
}
```

* Filed details

| Field          | Type    | Required | Description                                          | Possible values          |
|----------------|---------|----------|------------------------------------------------------|--------------------------|
| symbol         | String  | Y        | symbol to place order                                |                          | 
| clientOrderId  | String  | N        | client order id, max length is 40                    |                          |
| side           | Enum    | Y        | Order direction, Buy or Sell                         | BUY, SELL                | 
| quantity       | String  | Y        | Order quantity                                       |                          |
| price          | String  | depends  | price, required for limit order                      |                          | 
| type           | Enum    | -        | order type                                           | refer to orderType       | 
| timeInForce    | Enum    | -        | Time in force.                                       | GTC, FOK, IOC, POST_ONLY | 
| orderRespType  | ENUM    | -        | ACK and Result required immediate ack                | ACK,RESULT,NO_ACK        |

* Example Response:

```json
{
  "status": 0,
  "error": "OK",
  "data": {
    "orderId": 1521325650,
    "clientOrderId": "WEB9C6IB7h8absN",
    "symbol": "btcusdt",
    "cumQty": "0.000000",
    "cumQuote": "0.000000",
    "executedQty": "0.000000",
    "avgPrice": "0.000000",
    "origQty": "0.010000",
    "price": "426.000000",
    "side": "BUY",
    "timeInForce": "GTC",
    "type": "LIMIT",
    "status": "NEW"
  }
}
```

<a name="cancelsingleorder"/>

### Cancel Single Order by Order Id

* Request

```
DELETE /api/v1/private/spot-trade/cancel-order

{
  "symbol": "BTCUSDT",
  "orderId": "673681312"
}
```

* Response (the cancelled order, for example)

```
{
  "status": 0,
  "error": "OK",
  "data": {
    "orderId": 673681312,
    "clientOrderId": "WEB_SPOT_AxDGI18RKvw70",
    "symbol": "btcusdt",
    "cumQty": "0.00000000",
    "cumQuote": "0.00000000",
    "executedQty": "0.00000000",
    "avgPrice": "0.000000",
    "origQty": "0.02000000",
    "price": "2000.000000",
    "side": "BUY",
    "timeInForce": "GTC",
    "origType": "LIMIT",
    "type": "LIMIT",
    "activatePrice": null,
    "priceRate": null,
    "triggerPrice": "0.000000",
    "status": "NEW"
  }
}

```

<a name="cancelallorders"/>

### Cancel All Orders

* Request

```
POST /api/v1/private/spot-trade/cancel-all-order

{
  "symbol": null,
}
```

<a name="querytradeaccount"/>  

### Query Trading Account Balance

* Request

```  
GET /api/v1/private/spot-account/balance  
```  

* Response

```json  

{
  "status": 0,
  "error": "OK",
  "data": [
    {
      "currency": "ETH",
      "balance": 1.14063000,
      "balanceInTrading": 0,
      "lockedBalance": 0E-8,
      "availableBalance": 1.14063000
    },
    {
      "currency": "USDT",
      "balance": 3483.80478824,
      "balanceInTrading": 0,
      "lockedBalance": 2.14063000,
      "availableBalance": 348171.66415824
    }
  ]
}
```  

<a name="querytrades"/>  

### Query Recent Trades

rate limit weight: 5

* Request：

```  
GET /api/v1/private/spot-account/trade-histories?symbol=ETHUSDT&orderId=1212131  
```  

| Filed     | Type   | Description          |
|-----------|--------|----------------------|
| symbol    | String | Contract symbol name |  
| orderId   | Long   | order id             |  
| startTime | Long   | start time in millis |  
| endTime   | Long   | end time in millis   |  
| pageIndex | Int    | paging index         |  
| pageSize  | Int    | paging size          |  

* Response:

```json
{
  "status": 0,
  "error": null,
  "data": {
    "rows": [
      {
        "tradeId": 1729104,
        "userId": 100177,
        "orderId": 673376939,
        "symbol": "btcusdt",
        "side": "BUY",
        "fee": "-0.00001",
        "lastQty": "0.010",
        "lastPrice": "23926.2",
        "transactionTime": 1660141033566,
        "maker": false
      },
      {
        "tradeId": 1729102,
        "userId": 100177,
        "orderId": 673376680,
        "symbol": "btcusdt",
        "side": "BUY",
        "fee": "-0.00001",
        "lastQty": "0.010",
        "lastPrice": "23926.2",
        "transactionTime": 1660141029976,
        "maker": false
      },
      {
        "tradeId": 1729057,
        "userId": 100177,
        "orderId": 673359839,
        "symbol": "btcusdt",
        "side": "BUY",
        "fee": "-0.00001",
        "lastQty": "0.010",
        "lastPrice": "24004.0",
        "transactionTime": 1660140737581,
        "maker": false
      },
      ...
    ]
  }
}
```  

<a name="orderhistory"/>  

### Query Order History

rate limit weight: 5

* Request：

```  
GET /api/v1/private/spot-account/order-histories?symbol=BTCUSDT  
```  

* Request fields

| Field     | Type   | Description             |
|-----------|--------|-------------------------|
| symbol    | String | Contract symbol name    | 
| side      | ENUM   | Order Side, BUY or SELL | 
| orderId   | Long   | unique order id         | 
| status    | ENUM   | order status            | 
| startTime | Long   | start time in millis    |  
| endTime   | Long   | end time in millis      |  
| pageIndex | Int    | paging index            |  
| pageSize  | Int    | paging size             |

* Response:

```json
{
  "status": 0,
  "error": null,
  "data": {
    "rows": [
      {
        "orderId": 673686945,
        "clientOrderId": "WEB_SPOT_AxDGI18ssRKvw70",
        "symbol": "btcusdt",
        "type": "LIMIT",
        "origQty": "0.020",
        "executedQty": "0.000",
        "avgPrice": "0.0",
        "price": "2000.0",
        "timeInForce": "GTC",
        "side": "BUY",
        "transactTime": 1660147055838,
        "lastPriceAtPlace": "23996.8",
        "status": "CANCELED",
        "triggerPrice": "0.0",
        "baseCcy": "BTC"
      },
      ...
    ]
  }
}
```  

some field details:

| Filed     | Type     | Description               | Extra Description   |
|-----------|----------|---------------------------|---------------------|
| timestamp | Integer  | Timestamp in nanoseconds  |                     |  
| side      | String   | Trade side string         | BUY, SELL           |  
| price     | String   | Scaled trade price        |                     |  
| lastQty   | String   | trade size                |                     |  
| marker    | boolean  | marker or taker           |                     |  
| symbol    | String   | Contract symbol name      |                     |  
| avgPrice  | String   | average execution price   |                     |

<a name="websocketstream"/>  

# Websocket Stream:

<a name="wskline"/>  

## Kline/Candlestick Streams

The Kline/Candlestick Stream push updates to the current klines/candlestick every second.

**Kline/Candlestick chart intervals:**

m -> minutes; h -> hours; d -> days; w -> weeks; M -> months

* 1m
* 3m
* 5m
* 15m
* 30m
* 1h
* 2h
* 4h
* 6h
* 8h
* 12h
* 1d
* 3d
* 1w
* 1M

**Stream Name:** \<symbol\>@spotKline_\<interval\>

**Payload:**

```json
{
  "e": "kline",
  // Event type
  "E": 123456789,
  // Event time
  "s": "BTCUSDT",
  // contract name  
  "ct": "perpertual",
  // contract type
  "k": {
    "t": 123400000,
    // Kline start time
    "T": 123460000,
    // Kline close time   
    "i": "1m",
    // Interval
    "f": 100,
    // First trade ID
    "L": 200,
    // Last trade ID
    "o": "0.0010",
    // Open price
    "c": "0.0020",
    // Close price
    "h": "0.0025",
    // High price
    "l": "0.0015",
    // Low price
    "v": "1000",
    // Base asset volume
    "n": 100,
    // Number of trades
    "x": false,
    // Is this kline closed?
    "q": "1.0000",
    // Quote asset volume
    "V": "500",
    // Taker buy base asset volume
    "Q": "0.500"
    // Taker buy quote asset volume
  }
} 
```  

<a name="wsticker"/>  

## Ticker

**Stream Name:** <symbol>@spotTicker

**Payload:**

```json
{
  "e": "kline",
  // Event type
  "E": 123456789,
  // Event time
  "s": "BTCUSDT",
  // contract name  
  "ct": "perpertual",
  // contract type
  "k": {
    "t": 123400000,
    // Kline start time
    "T": 123460000,
    // Kline close time   
    "i": "1m",
    // Interval
    "f": 100,
    // First trade ID
    "L": 200,
    // Last trade ID
    "o": "0.0010",
    // Open price
    "c": "0.0020",
    // Close price
    "h": "0.0025",
    // High price
    "l": "0.0015",
    // Low price
    "v": "1000",
    // Base asset volume
    "n": 100,
    // Number of trades
    "x": false,
    // Is this kline closed?
    "q": "1.0000",
    // Quote asset volume
    "V": "500",
    // Taker buy base asset volume
    "Q": "0.500"
    // Taker buy quote asset volume
  }
}  
```  

<a name="wsorderbook"/>  

## Order Book

**Stream Name:** <symbol>@spotDepth<level>

level include:

* 10
* 20
* 50
* 100

**Payload:**

```json
{
  "e": "depthUpdate",
  // Event type
  "E": 123456789,
  // Event time
  "T": 123456788,
  // Transaction time 
  "s": "BTCUSDT",
  // Symbol
  "U": 157,
  // First update ID in event
  "u": 160,
  // Final update ID in event
  "pu": 149,
  // Final update Id in last stream(ie `u` in last stream)
  "b": [
    // Bids to be updated
    [
      "0.0024",
      // Price level to be updated
      "10"
      // Quantity
    ]
  ],
  "a": [
    // Asks to be updated
    [
      "0.0026",
      // Price level to be updated
      "100"
      // Quantity
    ]
  ]
}  
```  

<a name="wstrade"/>  

## Live Trade

**Stream Name:** <symbol>@spotTrade

**Payload:**

```json
{
  "e": "trade",
  "E": 1660148443864,
  "T": 1660148443856,
  "s": "btcusdt",
  "t": 2978355,
  "p": "24011.800000",
  "q": "0.07600000",
  "X": "MARKET",
  "m": false
}
```

<a name="accountstream"/>

## Account Stream

**Stream Name:** ${wsTokenFromApi}

for ws token management, please see [ws token management](#wstoken)

<a name="wsorderupdate">

**Order Update Payload:**

```json
{
  "at": "SPOT",
  "e": "ORDER_TRADE_UPDATE",
  "T": 1660148318743,
  "E": 1660148318753,
  "i": "170601",
  "o": {
    "ac": 1706010000,
    "t": 0,
    "x": "E_NEW",
    "L": "0.000000",
    "l": "0.00000000",
    "ap": "0.000000",
    "rq": "0.10000000",
    "z": "0.00000000",
    "n": "0.00000000",
    "i": 774963384,
    "c": "WEB_SPOT_mr1KWgAm3UVA",
    "C": 0,
    "s": "btcusdt",
    "S": "BUY",
    "o": "LIMIT",
    "is": false,
    "f": "GTC",
    "p": "2000.000000",
    "q": "0.10000000",
    "X": "NEW",
    "T": 1660148318743,
    "m": false,
    "cP": false,
    "lPP": "24042.100000",
    "tp": "0.000000"
  }
}
```

<a name="wsaccountupdate">

**Account Update Payload:**

account update can have two kinds of data: balance and position

```json
{
  "at": "SPOT",
  "e": "ACCOUNT_UPDATE",
  "T": 1660148359661230458,
  "E": 1660148359665,
  "i": "170601",
  "a": {
    "m": "ORDER",
    "B": [
      {
        "ccy": "USDT",
        "ab": "144259.84795386",
        "lb": "0.00000000",
        "vid": 93
      },
      {
        "ccy": "BTC",
        "ab": "0.13085900",
        "lb": "0.00000000",
        "vid": 9
      }
    ]
  }
}
```
