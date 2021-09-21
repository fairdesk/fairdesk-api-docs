

<a name="publicapi"/>

# Fairdesk Public API

<a name="general"/>

## General public API 

* Fairdesk provides HTTP Rest API for client to operate Orders, all endpoints return a JSON object.
* The default Rest API base endpoint is: **https://www.fairdesk.com/user/v1/** and **https://www.fairdesk.com/order/v1/**.  
* Fairdesk provides WebSocket API for client to receive market data, order and position updates. The WebSocket API url is: **wss://www.fairdesk.com/ws/**. 

<a name="restapi"/>

# REST API Standards

<a name="restresponse"/>

## Restful API Response

<a name="httpreturncodes"/>

### HTTP Return Codes

* HTTP `401` return code when unauthenticated
* HTTP `403` return code when lack of privilege.
* HTTP `429` return code when breaking a request rate limit.
* HTTP `5XX` return codes for Fairdesk internal errors. Note: This doesn't mean the operation failed, the execution status is **UNKNOWN** and could be Succeeded.

<a name="responseformat"/>

### Rest Response format

* All restful API shares the same response format.

```
{
    "status": <code>,
    "error": <msg>,
    "data": <data>
}

```

| Field | Description | 
|-------|------|
| code | 0 means `success`, non-zero means `error`|
| error  | when code is non-zero, it provides short error description |
| data | operation dependent  |
 

 
### Common constants

* order type

| order type | description |
|-----------|-------------|
| LIMIT | limit order |
| MARKET | market order |
| STOP_LOSS_MARKET | stop loss at market price |
| STOP_LOSS_LIMIT | stop loss at limit price |
| TAKE_PROFIT_LIMIT | take profit at limit price |
| TAKE_PROFIT_MARKET | take profit at market price |
| U_POST_ONLY | post only |
| CONDITIONAL_LIMIT | conditional limit order |
| CONDITIONAL_MARKET | conditional market order |


* order Status

| order status | description | 
|------------|-------------|
| NEW | order accepted  |
| PARTIALLY_FILLED | order partial filled |
| FILLED | order fully filled |
| CANCELED | order canceled |
| REPLACED | order been canceled and replaced by new one  |
| EXPIRED | Order expired |
 

* TimeInForce

| timeInForce | description |
|------------|-------------|
| GTC | good till cancel |
| POST_ONLY | post only |
| IOC | immediate or cancel |
| FOK | fill or kill |


* Price Trigger type

| trigger | description |
|------------|-------------|
| MARK_PRICE | trigger by mark price|
| LAST_PRICE | trigger by last price |
 

<a name="queryproductinfo"/>

#### Query Product Information

* Request：

```
GET /user/v1/public/instrument/products 
```

<a name="orderapilist"/>

### Trade API List

<a name="placeorder"/>

#### Place Order

* HTTP Request:

``` 
POST /order/v1/private/orders/place-order

{
  "quantity": "0.01",
  "price": "42601.5",
  "side": "BUY",
  "positionSide": "LONG",
  "isolated": true,
  "type": "LIMIT",
  "timeInForce": "GTC",
  "symbol": "btcusdt",
  "clientOrderId": "WEB9C6IB7h8absN",
  "orderRespType": "ACK"
}
```

| Field | Type | Required | Description | Possible values |
|-------|-------|--------|--------------|-----------------|
| symbol | String | Yes | Which symbol to place order |  | 
| clientOrderId | String | Yes | client order id, max length is 40| |
| side |  Enum | Yes | Order direction, Buy or Sell | BUY, SELL | 
| positionSide |  Enum | Yes | position direction | LONG, SHORT | 
| isolated |  Boolean | Yes | true for isolated position, false for cross position | true, false | 
| closePosition |  Boolean | No | indicate close position order or not | true, false | 
| quantity | String | Yes | Order quantity | |
| price | String | - | price, required for limit order | | 
| type | Enum | - | | refer to orderType | 
| timeInForce | Enum | - | Time in force. | GTC, FOK, IOC, POST_ONLY| 
| conditional | Boolean | - | indicate a conditional order | | 
| triggerType | Enum | - |  | LAST_PRICE, Mark_PRICE | 
| triggerPrice | String | - | Trigger Price | |
| tpTriggerType | String | - | take profit trigger type |  LAST_PRICE, Mark_PRICE  |
| tpTriggerPrice | String | - | take profit trigger price | |
| slTriggerType | String | - | stop loss trigger type |  LAST_PRICE, Mark_PRICE  |
| slTriggerPrice | String | - | stop loss trigger price | |
| orderRespType | ENUM | - | ACK and Result required order ack immediatel | ACK,RESULT,NO_ACK |


* HTTP Response:

```
{
  "status": 0,
  "error": "OK",
  "data": {
    "orderId": 1521325650,
    "clientOrderId": "WEB9C6IB7h8absN",
    "symbol": "btcusdt",
    "isolated": true,
    "cumQty": "0.000000",
    "cumQuote": "0.000000",
    "executedQty": "0.000000",
    "avgPrice": "0.000000",
    "origQty": "0.010000",
    "price": "426.000000",
    "side": "BUY",
    "positionSide": "LONG",
    "timeInForce": "GTC",
    "origType": "LIMIT",
    "type": "LIMIT",
    "activatePrice": null,
    "priceRate": null,
    "triggerPrice": "0.000000",
    "triggerType": "TRIGGER_NONE",
    "tpTriggerType": "TRIGGER_NONE",
    "tpTriggerPrice": "0.000000",
    "slTriggerType": "TRIGGER_NONE",
    "slTriggerPrice": "0.000000",
    "strategyParentId": 0,
    "status": "NEW"
  }
}
```

* Important fields description
    * Order average filled price,  inverse contract:`avgPrice = (cumQty/cumValueEv)/contractSize`; linear contract: `avgPrice = (cumValueEv/cumQty)/contractSize`. `contractSize` is fixed in [product](#marketapilist) api.

| Field | Description |
|------|----------|
| status | order status |
| cumQty | cumulative filled order quantity |
| orderId | internal unique order id |

<a name="cancelsingleorder"/>

#### Cancel Single Order

* Request
```
POST /order/v1/private/orders/cancel-order

{
  "symbol": "btcusdt",
  "orderId": 1521325650
}
```

* Response
    * Full Order

```
{
  "status": 0,
  "error": "OK",
  "data": {
    "orderId": 1521325650,
    "clientOrderId": "WEB9C6IB7h8absN",
    "symbol": "btcusdt",
    "isolated": true,
    "cumQty": "0.000000",
    "cumQuote": "0.000000",
    "executedQty": "0.000000",
    "avgPrice": "0.000000",
    "origQty": "0.010000",
    "price": "426.000000",
    "side": "BUY",
    "positionSide": "LONG",
    "timeInForce": "GTC",
    "origType": "LIMIT",
    "type": "LIMIT",
    "activatePrice": null,
    "priceRate": null,
    "triggerPrice": "0.000000",
    "triggerType": "TRIGGER_NONE",
    "tpTriggerType": "TRIGGER_NONE",
    "tpTriggerPrice": "0.000000",
    "slTriggerType": "TRIGGER_NONE",
    "slTriggerPrice": "0.000000",
    "strategyParentId": 0,
    "status": "NEW"
  }
}

```

* Response
    * Canceled orders

<a name="cancelall"/>

#### Cancel All Orders

* Request

```
POST /order/v1/private/orders/cancel-all-order

{
  "symbol": null,
  "settleCcy": "USDT"
}
```


<a name="querytradeaccount"/>

#### Query trading account and positions

* Request

```
GET /user/v1/private/account/asset
```
 
* Response

```json
{
  "status": 0,
  "error": "OK",
  "data": {
    "marginBalanceUsd": "1135.21",
    "marginBalanceBtc": "0.02659473",
    "totalAccountBalance": "1135.26",
    "totalUnRealizedPnL": "-0.06",
    "accounts": [
      {
        "currency": "USDT",
        "icon": "https://sgtnstatic-1306519353.cos.ap-singapore.myqcloud.com/currency/USDT.png",
        "accountBalance": "1135.26",
        "availBalance": "1133.14",
        "unRealizedPnL": "-0.06",
        "positionMargin": "2.12",
        "bonus": "0.00",
        "display": "Tether"
      }
    ]
  }
}
```

<b>Note</b> `unRealizedPnL` `totalUnRealizedPnL` and  needs to be calculated according to latest `markPrice`, formula is as below.


#### Query trading account and positions with unrealized-pnl

<a name="querycurrentpositions"/>

Below API presents unrealized pnl at `markprice` of positions with **considerable** cost, thus its [ratelimit](/Generic-API-Info.en.md#contractAPIGroup) weight is very high.

* Request

```
GET /user/v1/private/positions/current-positions

```

* Response

```
{
  "status": 0,
  "error": "OK",
  "data": [
    {
      "symbol": "btcusdt",
      "positionSide": "LONG",
      "isolated": true,
      "avgEntryPrice": "42745.50000000",
      "quantity": "0.00100000",
      "isolatedMargin": "2.12231500",
      "frozenMargin": "0.00000000",
      "margin": "",
      "liqPrice": "",
      "lastFundingTime": 0,
      "lastTxTime": 1632190125650,
      "baseCurrency": "BTC",
      "quoteCurrency": "USDT",
      "settleCurrency": "USDT",
      "curTermRealisedPnl": "0.00000000",
      "cumRealizedPnl": "-0.00500000"
    }
  ]
}

```

<a name="queryopenorders"/>

* Request
```
GET /user/v1/private/order/open-orders
```  
* Response

```
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
      "isolated": true,
      "side": "BUY",
      "positionSide": "LONG",
      "transactTime": 1632192208043,
      "status": "NEW",
      "markPriceAtPlace": "3013.71",
      "lastPriceAtPlace": null,
      "closePosition": false,
      "triggerPrice": "0.00",
      "triggerType": "TRIGGER_NONE",
      "tpTriggerType": "TRIGGER_NONE",
      "tpTriggerPrice": "0.00",
      "slTriggerType": "TRIGGER_NONE",
      "slTriggerPrice": "0.00",
      "strategyParentId": 0
    },
    {
      "orderId": 1523696056,
      "clientOrderId": "WEBxKKzzEZIkd0h",
      "symbol": "btcusdt",
      "type": "LIMIT",
      "origQty": "0.130",
      "executedQty": "0.000",
      "price": "426.0",
      "timeInForce": "GTC",
      "isolated": true,
      "side": "BUY",
      "positionSide": "LONG",
      "transactTime": 1632192194291,
      "status": "NEW",
      "markPriceAtPlace": "42782.6",
      "lastPriceAtPlace": null,
      "closePosition": false,
      "triggerPrice": "0.0",
      "triggerType": "TRIGGER_NONE",
      "tpTriggerType": "TRIGGER_NONE",
      "tpTriggerPrice": "0.0",
      "slTriggerType": "TRIGGER_NONE",
      "slTriggerPrice": "0.0",
      "strategyParentId": 0
    }
  ]
}
```

#### Change leverage

* Request

```
PUT https://testnet.fairdesk.com/order/v1/private/accounts/adjust-leverage

{
  "symbol": "ethusdt",
  "isolated": true,
  "leverage": "10"
}
```

| Field                | Type   | Description                                | Possible values |
|----------------------|--------|--------------------------------------------|--------------|
| symbol               | string | which position needs to change              |  |
| leverage             | integer   | unscaled leverage                       |  |
| isolated             | boolean   | isolated or crossed  |  |


* Response

```
{
  "status": 0,
  "error": "OK",
  "data": {
    "symbol": null,
    "isolated": true,
    "leverage": 10,
    "maxNotionalValue": 0
  }
}
```
 
<a name="adjustmargin"/>

#### Assign balances for isolated margin

* Request

```
PUT /order/v1/private/accounts/adjust-position-margin

{
  "symbol": "btcusdt",
  "positionSide": "LONG",
  "changeAmount": "0.1"
}
```


<a name="querytrades"/>

#### Query Recent Trades

* Request：
```
GET /user/v1/private/order/trade-histories?symbol=eth
```

| Field       | Type   | Description                                | 
|-------------|--------|--------------------------------------------|
| symbol      | String | Contract symbol name                       |

* Response:
```
{
  "status": 0,
  "error": null,
  "data": {
    "rows": [
      {
        "tradeId": 10485580,
        "userId": 100177,
        "orderId": 1522806127,
        "symbol": "btcusdt",
        "side": "SELL",
        "positionSide": "LONG",
        "fee": "-0.44894800",
        "lastQty": "0.030",
        "lastPrice": "42757.0",
        "realizedPnl": "1127.64000000",
        "isolated": false,
        "liquidationType": "L_NONE",
        "baseCcy": "BTC",
        "settledCcy": "USDT",
        "transactionTime": 1632191344624,
        "maker": false
      },
      ...
      ]
 }
}
```


<a name="queryorders"/>

#### Query Recent Orders

* Request：
```
GET /user/v1/private/order/order-histories?symbol=btc
```

| Field       | Type   | Description                                | 
|-------------|--------|--------------------------------------------|
| symbol      | String | Contract symbol name                       |

* Response:
```
{
  "status": 0,
  "error": null,
  "data": {
    "rows": [
      {
        "orderId": 1522808722,
        "clientOrderId": "WEBoWd7RAKV0fH0",
        "symbol": "btcusdt",
        "type": "LIMIT",
        "origQty": "0.100",
        "executedQty": "0.000",
        "avlPrice": "0.0",
        "price": "4211.0",
        "timeInForce": "GTC",
        "isolated": true,
        "side": "BUY",
        "positionSide": "LONG",
        "transactTime": 1632191376587,
        "markPriceAtPlace": "42744.9",
        "lastPriceAtPlace": "42757.0",
        "status": "CANCELED",
        "closePosition": false,
        "triggerPrice": "0.0",
        "triggerType": "TRIGGER_NONE",
        "tpTriggerType": "TRIGGER_NONE",
        "tpTriggerPrice": "0.0",
        "slTriggerType": "TRIGGER_NONE",
        "slTriggerPrice": "0.0",
        "strategyParentId": 0,
        "liqCounterpartyType": "L_NONE",
        "baseCcy": "BTC",
        "settledCcy": "USDT"
      },
      ...
      ]
 }
}
```

| Field       | Type   | Description                                | Possible values |
|-------------|--------|--------------------------------------------|--------------|
| timestamp   | Integer| Timestamp in nanoseconds                   |              |
| side        | String | Trade side string                          | BUY, SELL    |
| positionSide | String | Position Side                         | LONG, SHORT    |
| price     | String| Scaled trade price                         |              |
| lastQty        | String| trade size                          |              |
| marker    | boolean| marker or taker                   |              |
| symbol      | String | Contract symbol name                       |  |
| avlPrice      | String | average execution price                       |  |
   
  
#### Websocket Stream API:

## Live Subscribing/Unsubscribing to streams

* The following data can be sent through the websocket instance in order to subscribe/unsubscribe from streams. Examples can be seen below.
* The `id` used in the JSON payloads is an unsigned INT used as an identifier to uniquely identify the messages going back and forth.
* In the response, if the `result` received is `null` this means the request sent was a success for non-query requests (e.g. Subscribing/Unsubscribing).

### Subscribe to a stream
* Request
```
  {
    "method": "SUBSCRIBE",
    "params": [
      "btcusdt@depth10"
    ],
    "id": 1
  }
```

* Response
```
  {
    "result": null,
    "id": 1
  }
``` 

### Unsubscribe to a stream
* Request
 ```
  {
    "method": "UNSUBSCRIBE",
    "params": [
      "btcusdt@depth10"
    ],
    "id": 15
  }
 ```

* Response
  ```
  {
    "result": null,
    "id": 15
  }
  ```


### Listing Subscriptions
* Request
  ```
  {
    "method": "LIST_SUBSCRIPTIONS",
    "id": 4
  }
  ```

* Response
  ```
  {
    "result": [
      "btcusdt@depth10"
    ],
    "id": 4
  }
  ```

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

**Stream Name:** \<symbol\>@kline_\<interval\>


**Payload:**
```
{
  "e": "kline",     // Event type
  "E": 123456789,   // Event time
  "s": "BNBBTC",    // Symbol
  "k": {
    "t": 123400000, // Kline start time
    "T": 123460000, // Kline close time
    "s": "BNBBTC",  // Symbol
    "i": "1m",      // Interval
    "f": 100,       // First trade ID
    "L": 200,       // Last trade ID
    "o": "0.0010",  // Open price
    "c": "0.0020",  // Close price
    "h": "0.0025",  // High price
    "l": "0.0015",  // Low price
    "v": "1000",    // Base asset volume
    "n": 100,       // Number of trades
    "x": false,     // Is this kline closed?
    "q": "1.0000",  // Quote asset volume
    "V": "500",     // Taker buy base asset volume
    "Q": "0.500",   // Taker buy quote asset volume
  }
}
```

<a name="wsmarkprice"/>

## Mark Price Streams
Mark price and funding rate for a single symbol pushed every second.

**Stream Name:** <symbol>@markPrice

**Payload:**
```
  {
        "e": "markPriceUpdate",     // Event type
        "E": 1632195394014,         // Event time
        "s": "ETHUSDT",             // Symbol
        "p": "11794.15000000",      // Mark price
        "i": "11784.62659091",      // Index price
        "P": "11784.25641265",      // Estimated Settle Price, only useful in the last hour before the settlement starts
        "r": "0.00038167",          // Funding rate
        "T": 1632211200000          // Next funding time
}
```


<a name="wsticker"/>

## ticker

**Stream Name:** <symbol>@ticker

**Payload:**
```
{
  "e": "24hrTicker",  // Event type
  "E": 123456789,     // Event time
  "s": "BTCUSDT",     // Symbol
  "p": "0.0015",      // Price change
  "P": "250.00",      // Price change percent
  "w": "0.0018",      // Weighted average price
  "c": "0.0025",      // Last price
  "Q": "10",          // Last quantity
  "o": "0.0010",      // Open price
  "h": "0.0025",      // High price
  "l": "0.0010",      // Low price
  "v": "10000",       // Total traded base asset volume
  "q": "18",          // Total traded quote asset volume
  "O": 0,             // Statistics open time
  "C": 86400000,      // Statistics close time
  "F": 0,             // First trade ID
  "L": 18150,         // Last trade Id
  "n": 18151          // Total number of trades
}
```



<a name="wsorderbook"/>

## order book

**Stream Name:** <symbol>@depth<level>

level include: 
* 10
* 20
* 100


**Payload:**
``` 
{
    "e": "depthUpdate", // Event type
        "E": 123456789,     // Event time
        "T": 123456788,     // Transaction time 
        "s": "BTCUSDT",     // Symbol
        "U": 157,           // First update ID in event
        "u": 160,           // Final update ID in event
        "pu": 149,          // Final update Id in last stream(ie `u` in last stream)
        "b": [              // Bids to be updated
        [
            "0.0024",       // Price level to be updated
            "10"            // Quantity
        ]
    ],
        "a": [              // Asks to be updated
        [
            "0.0026",       // Price level to be updated
            "100"          // Quantity
        ]
    ]
}
```


<a name="wstrade"/>

## live trade

**Stream Name:** <symbol>@trade


**Payload:**
``` 
{
    "e": "trade", // Event type
        "E": 123456789, // Event time
        "T": 123456788, // Transaction time
        "s": "BTCUSDT", // Symbol
        "t": 157, // update id
        "p":  "60221.0", // price
        "q": "0.01",  // quantity 
        "X": "Market", //Trade type
        "m": true,  // is buyer maker 
}
```
 
