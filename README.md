# Table of Contents * [Fairdesk Public API](#publicapi) * [General Public API Information](#general)

* [REST API Standards](#restapi)
    * [HTTP Restful Response](#restresponse)
        * [HTTP Return Codes](#httpreturncodes)
        * [HTTP Restful Response Format](#responseformat)
        * [Restful Response Error Codes](#errorcode)
    * [HTTP REST Request Header](#httprestheader)
    * [API Rate Limits](#apiratelimits)
    * [Endpoint Security Type](#securitytype)
        * [Signature Example 1: HTTP GET Request](#signatureexample1)
        * [Signature Example 2: HTTP POST Request](#signatureexample2)
    * [Request/Response Field Explained](#fieldexplained)
        * [Common Constants](#commconsts)
            * [Order Side](#orderside)
            * [Position Side](#positionside)
            * [Order Type](#ordertype)
            * [Order Status](#orderstatus)
            * [Time in Force](#timeinforce)
            * [Price Trigger Type](#pricetriggertype)
    * [REST API List](#restapilist)
        * [Query Product Information](#queryproductinfo)
        * [Query Current Future Positions](#querycurrentpositions)
        * [List Open Orders](#queryopenorders)
        * [Place Order](#placeorder)
        * [Cancel Single Order by Order id](#cancelsingleorder)
        * [Cancel All Orders](#cancelallorders)
        * [Query Trading Account Balance](#querytradeaccount)
        * [Feature Account Symbol Config](#symbolconfig)
        * [Adjust leverage](#adjustleverage)
        * [Query Transaction History](#querytransactions)
        * [Assign Balances for Isolated Margin](#adjustmargin)
        * [Query Recent Trades](#querytrades)
        * [Query Order History](#orderhistory)
* [Websocket Stream API](#websocket-stream)
    * [Live Subscribing/Unsubscribing to streams](#stream)
    * [Subscribe to a Stream](#substream)
    * [Unsubscribe to a Stream](#unsubstream)
    * [Listing Subscriptions](#listsub)
    * [Kline/Candlestick Streams](#wskline)
    * [Mark Price Streams](#wsmarkprice)
    * [Ticker](#wsticker)
    * [Order Book](#wsorderbook)
    * [Live Trade](#wstrade)
    * [Websocket Token](#wstoken)
        * [Create WebSocket Token](#createwstoken)
        * [Refresh WebSocket Token](#refreshwstoken)
        * [Delete WebSocket Token](#deletewstoken)
    * [Account Stream](#accountstream)
        * [Order Update](#wsorderupdate)
        * [Account Update](#wsaccountupdate)

<a name="publicapi"/>

# Fairdesk Public API

<a name="general"/>

## General API Information

* Fairdesk provides HTTP Rest API for client to operate Orders, all endpoints return a JSON object.
* The default Rest API base endpoint is: **https://api.fairdesk.com**.
* Fairdesk provides WebSocket API for client to receive market data, order and position updates.
* The WebSocket API url is: **wss://www.fairdesk.com/**.

<a name="restapi"/>

# REST API Standards

<a name="restresponse"/>

## Restful API Response

<a name="httpreturncodes"/>

### HTTP Return Codes

- HTTP `401` return code when unauthenticated
- HTTP `403` return code when lack of privilege.
- HTTP `429` return code when breaking a request rate limit.
- HTTP `5XX` return codes for Fairdesk internal errors. Note: This doesn't mean the operation failed, the execution
  status is **UNKNOWN** and could be Succeeded.

<a name="responseformat"/>

### Rest Response Format

* All restful API except ***starting*** with `/md` shares same response format.

```
{
    "status": <code>,
    "error": <msg>,
    "data": <data>
}
```

| Field | Description                                                |
| ----- | ---------------------------------------------------------- |
| code  | 0 means `success`, non-zero means `error`                  |
| error | when code is non-zero, it provides short error description |
| data  | operation dependent                                        |

<a name="errorcode"/>

### Error Codes

[Trading Error Codes](TradingErrorCode.md)

<a name="httprestheader"/>

## HTTP REST Request Header

Every PRIVATE HTTP Request must have the following Headers:

* x-fairdesk-access-key: This is ***API-KEY*** (id field) from Fairdesk site.
* x-fairdesk-request-expiry : This describes the Unix   ***EPoch milliseconds***  to expire the request, normally it
  should be (Now() + 1 minute)
* x-fairdesk-request-signature : This is HMAC SHA256 signature of the http request. Secret is ***API Secret***, its
  formula is : HMacSha256( URL Path + QueryString + Expiry + body )

<a name="apiratelimits"/>

## API Rate Limit Rules

* private API requests are subject to 200 limit quota per minute
* Every private API call consumes 1 quota

<a name="securitytype"/>

## Endpoint Security Type

* Each private API call must be signed and pass to server in HTTP header `x-fairdesk-request-signature`.
* Endpoints use `HMAC SHA256` signatures. The `HMAC SHA256 signature` is a keyed `HMAC SHA256` operation. Use
  your `apiSecret` as the key and the string `(URL Path + QueryString + Expiry + body)` as the value for the HMAC
  operation.
* `apiSecret` = `Base64::urlDecode(API Secret)`
* The `signature` is **case sensitive**.

<a name="signatureexample1"/>

### Signature Example 1: HTTP GET Request

* API REST Request URL: https://api.fairdesk.com/api/v1/private/account/symbol-config
* Request Path: /api/v1/private/account/symbol-config
* Request Query: <null>
* Request Body: <null>
* Request Expiry: 1649999999999
* Signature: HMacSha256(/api/v1/private/account/symbol-config + 1649999999999)

<a name="signatureexample2"/>

### Signature Example 2: HTTP POST Request

* API REST Request URL: https://api.fairdesk.com/api/v1/private/account/config/adjust-leverage
* Request Path: /api/v1/private/account/config/adjust-leverage
* Request Query: <null>
* Request Body: {  "symbol": "btcusdt",  "isolated": true,  "leverage": "120"}
* Request Expire: 1649999999999
* Signature: HMacSha256(/api/v1/private/account/config/adjust-leverage + 1649999999999 + {  "symbol": "btcusdt",  "
  isolated": true,  "leverage": "120"})

<a name="fieldexplained"/>

## Request/Response Field Explained

<a name="commconsts"/>

### Common Constants

<a name="orderside"/>

#### Order Side

| type | description |
|-----------|-------------|
| BUY       | BUY         |
| SELL      | SELL        |

<a name="positionside"/>

#### Position Side

| type  | description    |
|-------|----------------|
| LONG  | LONG position  |
| SHORT | SHORT position |

<a name="ordertype"/>

#### Order Type

| order type         | description                 |
|--------------------|-----------------------------|
| LIMIT              | limit order                 |
| MARKET             | market order                |
| STOP_LOSS_MARKET   | stop loss at market price   |
| STOP_LOSS_LIMIT    | stop loss at limit price    |
| TAKE_PROFIT_LIMIT  | take profit at limit price  |
| TAKE_PROFIT_MARKET | take profit at market price |
| POST_ONLY          | post only                   |
| CONDITIONAL_LIMIT  | conditional limit order     |
| CONDITIONAL_MARKET | conditional market order    |

<a name="orderstatus"/>

#### Order Status

| order status     | description                                 | 
|------------------|---------------------------------------------|
| NEW              | order accepted                              |
| PARTIALLY_FILLED | order partial filled                        |
| FILLED           | order fully filled                          |
| CANCELED         | order canceled                              |
| REPLACED         | order been canceled and replaced by new one |
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

<a name="pricetriggertype"/>

#### Price Trigger Type

| trigger    | description           |
|------------|-----------------------|
| MARK_PRICE | trigger by mark price |
| LAST_PRICE | trigger by last price |

<a name="restapilist"/>

## REST Contract Group API List

<a name="queryproductinfo"/>

### Query Product Information

* Request：

```
GET /api/v1/public/products/products 
```

* Example Response:

```json
{
  "status": 0,
  "error": "OK",
  "data": [
    {
      "symbolId": 1211,
      "symbol": "btcusdt",
      "displayName": "BTC/USDT",
      "baseCurrency": "BTC",
      "quoteCurrency": "USDT",
      "settleCurrency": "USDT",
      "type": "FUTURE_PERPETUAL",
      "productType": "Perpetual",
      "tickSize": "0.500000",
      "stepSize": "0.001000",
      "maxPrice": "500000.000000",
      "minPrice": "0.500000",
      "maxOrderQty": "100.000000",
      "minOrderQty": "0.000000",
      "marketMaxQty": "0.000000",
      "priceScale": 6,
      "valueScale": 6,
      "ratioScale": 8,
      "defaultLeverage": 20,
      "priceDecimal": 1,
      "amountDecimal": 3,
      "fundingInterval": "Every 8 hours",
      "makerFeeRate": "0.0001",
      "takerFeeRate": "0.00015",
      "marketPriceDiffRate": 0.1,
      "limitPriceDiffRate": 0.1,
      "strategyPriceDiffRate": 0.075,
      "liquidationFeeRate": 0.01,
      "status": "TRADING"
    },
    ...
  ]
}

```

* Detail for the Response Field:

| Field           | Type           | Description                       | Possible values      |
|-----------------|----------------|-----------------------------------|----------------------|
| symbolId        | Int            | internal symbol id                |                      |
| symbol          | String         | internal symbol id                |                      |
| tickSize        | Decimal        | minimum price incremental size    |                      |
| stepSize        | Decimal        | minimum quantity incremental size |                      |
| maxPrice        | Decimal        | Max price                         |                      |
| minPrice        | Decimal        | min price                         |                      |
| maxOrderQty     | Decimal        | min price                         |                      |
| minOrderQty     | Decimal        | min order size                    |                      |
| defaultLeverage | Int            | default leverage                  |                      |
| makerFeeRate    | Decimal        | default maker fee rate            |                      |
| takerFeeRate    | Decimal        | default taker fee rate            |                      |
| status          | Enum           | TRADING                           | TRADING,SUSPEND,STOP |

<a name="querycurrentpositions"/>

### Query Current Future Positions

* Request

```
GET /api/v1/private/account/positions
```

* Example Response

```json
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
      "lastTxTime": 1632190125650,
      "baseCurrency": "BTC",
      "quoteCurrency": "USDT",
      "settleCurrency": "USDT"
    }
  ]
}
```

* Response Field:

| Field          | Type    | Description                                            | Possible values |
|----------------|---------|--------------------------------------------------------|-----------------|
| isolated       | Boolean | true for isolated position, false for crossed position | true, false     |
| isolatedMargin | Decimal | margin used for isolated position                      |                 |
| positionSide   | String  | Position Side                                          | LONG, SHORT     |
| avgEntryPrice  | Decimal | averaged entry price                                   |                 |
| quantity       | Decimal | position size                                          |                 |
| symbol         | String  | Trading pair                                           |                 |
| lastTxTime     | Long    | timestamp for last transaction                         |                 |

<a name="queryopenorders"/>

### List Open Orders

* Request

```
GET /api/v1/private/trade/open-orders
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
      "isolated": true,
      "side": "BUY",
      "positionSide": "LONG",
      "transactTime": 1632192208043,
      "status": "NEW",
      "closePosition": false,
      "triggerPrice": "0.00",
      "triggerType": "TRIGGER_NONE",
      "tpTriggerType": "TRIGGER_NONE",
      "tpTriggerPrice": "0.00",
      "slTriggerType": "TRIGGER_NONE",
      "slTriggerPrice": "0.00",
      "strategyParentId": 0
    },
    ...
  ]
}
```

* Explanation for the Fields

| Field          | Type      | Description                                           | Possible values             |
|----------------|-----------|-------------------------------------------------------|-----------------------------|
| symbol         | String    | Which symbol to place order                           |                             | 
| clientOrderId  | String    | client order id, max length is 40                     |                             |
| orderId        | Long      | unique order id                                       |                             |
| status         | Enum      | order execution status                                | NEW,PARTIALLY_FILLED,FILLED |
| side           | Enum      | Order direction, Buy or Sell                          | BUY, SELL                   | 
| positionSide   | Enum      | position direction                                    | LONG, SHORT                 | 
| transactTime   | Long      | last transaction time                                 |
| isolated       | Boolean   | true for isolated position, false for cross position  | true, false                 | 
| closePosition  | Boolean   | indicate close position order or not                  | true, false                 | 
| quantity       | String    | Order quantity                                        |                             |
| price          | String    | price, required for limit order                       |                             | 
| type           | Enum      | order type                                            | refer to orderType          | 
| timeInForce    | Enum      | Time in force.                                        | GTC, FOK, IOC, POST_ONLY    | 
| conditional    | Boolean   | indicate a conditional order                          |                             | 
| triggerType    | Enum      | condition trigger type                                | LAST_PRICE, Mark_PRICE      | 
| triggerPrice   | String    | Trigger Price                                         |                             |
| tpTriggerType  | String    | take profit trigger type                              | LAST_PRICE, Mark_PRICE      |
| tpTriggerPrice | String    | take profit trigger price                             |                             |
| slTriggerType  | String    | stop loss trigger type                                | LAST_PRICE, Mark_PRICE      |
| slTriggerPrice | String    | stop loss trigger price                               |                             |
| origQty        | Decimal   | original order quantity                               |                             |
| executedQty    | Decimal   | executed quantity                                     |                             |
| activatePrice  | Decimal   | activated price from the `parent` conditional order   |                             |

<a name="placeorder"/>

### Place Order

* HTTP Request:

```
POST /api/v1/private/trade/place-order

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

* Filed details

| Field          | Type    | Required | Description                                          |  Possible values         |
|----------------|---------|----------|------------------------------------------------------|--------------------------|
| symbol         | String  | Y        | symbol to place order                                |                          | 
| clientOrderId  | String  | N        | client order id, max length is 40                    |                          |
| side           | Enum    | Y        | Order direction, Buy or Sell                         | BUY, SELL                | 
| positionSide   | Enum    | Y        | position direction                                   | LONG, SHORT              | 
| isolated       | Boolean | Y        | true for isolated position, false for cross position | true, false              | 
| closePosition  | Boolean | N        | indicate close position order or not                 | true, false              | 
| quantity       | String  | Y        | Order quantity                                       |                          |
| price          | String  | depends  | price, required for limit order                      |                          | 
| type           | Enum    | -        | order type                                           | refer to orderType       | 
| timeInForce    | Enum    | -        | Time in force.                                       | GTC, FOK, IOC, POST_ONLY | 
| conditional    | Boolean | -        | indicate a conditional order                         |                          | 
| triggerType    | Enum    | -        | trigger type                                         | LAST_PRICE, Mark_PRICE   | 
| triggerPrice   | String  | -        | Trigger Price                                        |                          |
| tpTriggerType  | String  | -        | take profit trigger type                             | LAST_PRICE, Mark_PRICE   |
| tpTriggerPrice | String  | -        | take profit trigger price                            |                          |
| slTriggerType  | String  | -        | stop loss trigger type                               | LAST_PRICE, Mark_PRICE   |
| slTriggerPrice | String  | -        | stop loss trigger price                              |                          |
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
    "triggerPrice": "0.000000",
    "triggerType": "TRIGGER_NONE",
    "tpTriggerType": "TRIGGER_NONE",
    "tpTriggerPrice": "0.000000",
    "slTriggerType": "TRIGGER_NONE",
    "slTriggerPrice": "0.000000",
    "status": "NEW"
  }
}
```

<a name="cancelsingleorder"/>

### Cancel Single Order by Order Id

* Request

```
POST /api/v1/private/trade/cancel-order

{
  "symbol": "btcusdt",
  "orderId": 1521325650
}
```

* Response (the cancelled order, for example)

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

<a name="cancelallorders"/>

### Cancel All Orders

* Request

```
POST /api/v1/private/trade/cancel-all-order

{
  "symbol": null,
  "settleCcy": "USDT"
}
```

<a name="querytradeaccount"/>  

### Query Trading Account Balance

* Request

```  
GET /api/v1/private/account/balance  
```  

* Response

```json  
{
  "status": 0,
  "error": "OK",
  "data": {
    "marginBalanceUsd": "43.79",
    "marginBalanceBtc": "0.00103591",
    "totalAccountBalance": "49.27",
    "totalUnRealizedPnL": "-5.48",
    "accounts": [
      {
        "currency": "USDT",
        "accountBalance": "49.27",
        "availBalance": "33.22",
        "unRealizedPnL": "-5.48",
        "positionMargin": "8.46",
        "bonus": "50.00",
        "display": "Tether"
      }
    ]
  }
}  
```  

<b>Note</b> `unRealizedPnL` `totalUnRealizedPnL` and needs to be calculated according to latest `markPrice`.

<a name="symbolconfig"/>

### Feature Account Symbol Config

* Request

```
GET /api/v1/private/account/symbol-config
```

* Example Response

```json

{
  "status": 0,
  "error": "OK",
  "data": [
    {
      "symbol": "adausdt",
      "crossLeverage": 20,
      "isolatedLeverage": 20,
      "makerFeeRate": "0.00018",
      "takerFeeRate": "0.00028"
    },
    ...
  ]
}
```

<a name="adjustleverage"/>

### Adjust leverage

* Request

```  
PUT  /api/v1/private/account/config/adjust-leverage  
  
{  
 "symbol": "ethusdt", "isolated": true, "leverage": "10"}  
```  

| Field     | Type    | Description                    | Possible Values |  
|-----------|---------|--------------------------------|-----------------|
| symbol    | string  | which position needs to change |                 |  
| leverage  | integer | unscaled leverage              |                 |  
| isolated  | boolean | isolated or crossed            |                 |  

* Response

```json
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

### Assign Balances for Isolated Margin

* Request

```  
PUT /api/v1/private/account/config/adjust-position-margin  
  
{  
 "symbol": "btcusdt", "positionSide": "LONG", "changeAmount": "0.1"
}  
```  

<a name="querytrades"/>  

### Query Recent Trades

* Request：

```  
GET /api/v1/private/account/trade-histories?symbol=ETHUSDT&orderId=1212131  
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

<a name="orderhistory"/>  

### Query Order History

* Request：

```  
GET /api/v1/private/account/order-histories?symbol=BTCUSDT  
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

some field details:

| Filed        | Type     | Description               | Extra Description   |
|--------------|----------|---------------------------|---------------------|
| timestamp    | Integer  | Timestamp in nanoseconds  |                     |  
| side         | String   | Trade side string         | BUY, SELL           |  
| positionSide | String   | Position Side             | LONG, SHORT         |  
| price        | String   | Scaled trade price        |                     |  
| lastQty      | String   | trade size                |                     |  
| marker       | boolean  | marker or taker           |                     |  
| symbol       | String   | Contract symbol name      |                     |  
| avlPrice     | String   | average execution price   |                     |

<a name="querytransactions"/>  

### Query Transaction History

* Request：

```  
GET /api/v1/private/account/transaction-histories?type=Transfer&symbol=BTCUSDT  
```  

* Request fields

| Field     | Type   | Description                                                                  |
|-----------|--------|------------------------------------------------------------------------------|
| symbol    | String | Contract symbol name                                                         | 
| type      | ENUM   | Transaction Type: Transfer,RealizedPnl,FundingFee,Commission, InsuranceClear | 
| startTime | Long   | start time in millis                                                         |  
| endTime   | Long   | end time in millis                                                           |  
| pageIndex | Int    | paging index                                                                 |  
| pageSize  | Int    | paging size                                                                  |

* Example Response

```json

{
  "status": 0,
  "error": null,
  "data": {
    "rows": [
      {
        "tranId": 1649664000017423918,
        "userId": 100177,
        "orderId": 0,
        "symbol": "btcusdt",
        "currency": "USDT",
        "type": "FundingFee",
        "businessType": null,
        "amount": "-0.02",
        "transactionTime": 1649664000018
      },
      ...
    ],
    "total": 6617
  }
}
```

<a name="websocketstream"/>  

# Websocket Stream:

<a name="stream"/>

## Live Subscribing/Unsubscribing to streams

* The following data can be sent through the websocket instance in order to subscribe/unsubscribe from streams. Examples
  can be seen below.
* The `id` used in the JSON payloads is an unsigned INT used as an identifier to uniquely identify the messages going
  back and forth.
* In the response, if the `result` received is `null` this means the request sent was a success for non-query requests (
  e.g. Subscribing/Unsubscribing).

<a name="substream"/>

### Subscribe to a stream

* Request

```  
 { "method": "SUBSCRIBE", "params": [ "btcusdt@depth10" ], "id": 1 }
 ```  

* Response

```  
 { "result": null, "id": 1 }
 ```   

<a name="unsubstream"/>

### Unsubscribe to a stream

* Request

 ``` 
 { "method": "UNSUBSCRIBE", "params": [ "btcusdt@depth10" ], "id": 15 } 
 ```  

* Response

```  
 { "result": null, "id": 15 } 
 ```  

<a name="listsub"/>

### Listing Subscriptions

* Request

```  
 { "method": "LIST_SUBSCRIPTIONS", "id": 4 } 
```  

* Response

```  
 { "result": [ "btcusdt@depth10" ], "id": 4 } 
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

```json
{
  "e": "kline", // Event type
  "E": 123456789, // Event time
  "s": "BTCUSDT", // contract name  
  "ct": "perpertual", // contract type
  "k": {
    "t": 123400000, // Kline start time
    "T": 123460000, // Kline close time   
    "i": "1m", // Interval
    "f": 100, // First trade ID
    "L": 200, // Last trade ID
    "o": "0.0010", // Open price
    "c": "0.0020", // Close price
    "h": "0.0025", // High price
    "l": "0.0015", // Low price
    "v": "1000", // Base asset volume
    "n": 100, // Number of trades
    "x": false, // Is this kline closed?
    "q": "1.0000", // Quote asset volume
    "V": "500", // Taker buy base asset volume
    "Q": "0.500" // Taker buy quote asset volume
  }
} 
```  

<a name="wsmarkprice"/>  

## Mark Price Streams

Mark price and funding rate for a single symbol pushed every second.

**Stream Name:** <symbol>@markPrice

**Payload:**

```json
   {
  "e": "kline", // Event type
  "E": 123456789, // Event time
  "s": "BTCUSDT", // contract name  
  "ct": "perpertual", // contract type
  "k": {
    "t": 123400000, // Kline start time
    "T": 123460000, // Kline close time   
    "i": "1m", // Interval
    "f": 100, // First trade ID
    "L": 200, // Last trade ID
    "o": "0.0010", // Open price
    "c": "0.0020", // Close price
    "h": "0.0025", // High price
    "l": "0.0015", // Low price
    "v": "1000", // Base asset volume
    "n": 100, // Number of trades
    "x": false, // Is this kline closed?
    "q": "1.0000", // Quote asset volume
    "V": "500", // Taker buy base asset volume
    "Q": "0.500" // Taker buy quote asset volume
  }
}
```  

<a name="wsticker"/>  

## Ticker

**Stream Name:** <symbol>@ticker

**Payload:**

```json
{
  "e": "kline", // Event type
  "E": 123456789, // Event time
  "s": "BTCUSDT", // contract name  
  "ct": "perpertual", // contract type
  "k": {
    "t": 123400000, // Kline start time
    "T": 123460000, // Kline close time   
    "i": "1m", // Interval
    "f": 100, // First trade ID
    "L": 200, // Last trade ID
    "o": "0.0010", // Open price
    "c": "0.0020", // Close price
    "h": "0.0025", // High price
    "l": "0.0015", // Low price
    "v": "1000", // Base asset volume
    "n": 100, // Number of trades
    "x": false, // Is this kline closed?
    "q": "1.0000", // Quote asset volume
    "V": "500", // Taker buy base asset volume
    "Q": "0.500" // Taker buy quote asset volume
  }
}  
```  

<a name="wsorderbook"/>  

## Order Book

**Stream Name:** <symbol>@depth<level>

level include:

* 10
* 20
* 50
* 100

**Payload:**

```json
{
  "e": "depthUpdate", // Event type
  "E": 123456789, // Event time
  "T": 123456788, // Transaction time 
  "s": "BTCUSDT", // Symbol
  "U": 157, // First update ID in event
  "u": 160, // Final update ID in event
  "pu": 149, // Final update Id in last stream(ie `u` in last stream)
  "b": [// Bids to be updated
    [
      "0.0024", // Price level to be updated
      "10" // Quantity
    ]
  ],
  "a": [// Asks to be updated
    [
      "0.0026", // Price level to be updated
      "100" // Quantity
    ]
  ]
}  
```  

<a name="wstrade"/>  

## Live Trade

**Stream Name:** <symbol>@trade

**Payload:**

```json
{
  "e": "trade", // Event type
  "E": 123456789, // Event time
  "T": 123456788, // Transaction time
  "s": "BTCUSDT", // Symbol
  "t": 157, // update id
  "p": "60221.0", // price
  "q": "0.01", // quantity 
  "X": "Market", //Trade type
  "m": true // is buyer maker 
}  
```

<a name="wstoken"/>

## Websocket Token

<a name="createwstoken"/>

### Create WebSocket Token

* Request

```  
POST /api/v1/private/token/create
```

* Example Response

```json
{
  "status": 0,
  "error": "OK",
  "data": {
    "apiKey": "YourAPIKeyName",
    "wsToken": "yourAPIWsToken",
    "expiry": 1649673693595
  }
}
```

this method is idempotent, call this method multiple times will generate the same websocket token


<a name="refreshwstoken"/>

### Refresh WebSocket Token

* Request

```  
POST /api/v1/private/token/refresh
```

If no upstanding ws token is valid, an error of "error-code.api.ws-token.not-created" is thrown. 

* Example Response

```json
{
  "status": 0,
  "error": "OK",
  "data": {
    "apiKey": "YourAPIKeyName",
    "wsToken": "yourAPIWsToken",
    "expiry": 1649673693595
  }
}
```


<a name="deletewstoken"/>

### Delete WebSocket Token

* Request

```  
DELETE /api/v1/private/token/delete
```

* Success Response

```json
{
  "status": 0,
  "error": "OK",
  "data": true
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
  "e": "ORDER_TRADE_UPDATE",
  "T": 1649678235588,
  "E": 1649678235591,
  "o": {
    "ac": 1106010011, // accountId
    "x": "E_NEW",   // execution type
    "L": "0.000000", // last Price
    "l": "0.000000",  // last quantity
    "ap": "40996.000000", // average price
    "rq": "0.000000",   // remainingQty
    "z": "0.017000",   // cumFilledQty
    "n": "0.000000",    // commission
    "i": 79519757563,   // order Id
    "c": "WEB__1WNgGe_IGbm", // client order Id
    "C": 0,           // cancel Id
    "s": "btcusdt",    // symbol
    "S": "BUY",        // order side
    "ps": "LONG",      // position side
    "o": "MARKET",      // order type
    "is": true,         // isolated
    "f": "IOC",          // time in force
    "p": "0.000000",     // order price
    "q": "0.017000",     // order quantity
    "X": "NEW",          // order status
    "T": 1649678235588,   // order trade time
    "m": false,          // is maker
    "cP": false,         // close position or not
    "tp": "0.000000",    // trigger price
    "tt": "TRIGGER_NONE", // price trigger type, TRIGGER_NONE,LAST_PRICE,MARK_PRICE 
    "tpType": "TRIGGER_NONE", // take profit trigger type
    "tpPrice": "0.000000",   // take profit trigger price
    "slType": "TRIGGER_NONE", // stop loss  trigger type
    "slPrice": "0.000000", // stop loss trigger price
    "sPid": 0
  }
}
```

<a name="wsaccountupdate">

**Account Update Payload:**

account update can have two kinds of data: balance and position 

```json
{
  "e": "ACCOUNT_UPDATE",
  "T": 1649678235588338434,
  "E": 1649678235591,
  "i": "110601", // userId
  "a": {
    "m": "ORDER",
    "B": [            // balance
      {
        "ccy": "USDT",  // currency
        "ab": "39.702997", // account balance
        "cb": "31.095886", // cross balance
        "vid": 7548 // account version Id
      }
    ],
    "P": [            // position
      {
        "a": 1106010011, // accountId
        "ccy": "USDT",// currency
        "s": "btcusdt",// symbol
        "ps": "LONG", // position side
        "is": true,  // isolated or not
        "pa": "0.017000", // position amount
        "ep": "40996.000000", // averageEntryPrice
        "cr": "4.994039", // cumRealizedPnl
        "im": "8.607111",  // isolated frozenMargin
        "vid": 1008, // version Id
        "T": 1649678235588 // trade Time
      }
    ]
  }
}
```
