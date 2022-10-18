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
* Fairdesk provides WebSocket API for client to receive market data, order and position updates.
* The default Rest API base endpoint is: **https://api.fairdesk.com**.
* The WebSocket API url is: **wss://www.fairdesk.com/**.
* The Testnet endpoints are **https://api-testnet.fairdesk.com** and **wss://testnet.fairdesk.com**

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

[Trading Error Codes](TradeErrorCode.md)

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
 
