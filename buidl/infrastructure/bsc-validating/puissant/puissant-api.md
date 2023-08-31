---
description: Designed as Standard RPC Style Endpoint
---

# API

Base url if not specified: `https://puissant-bsc.48.club`

{% swagger method="post" path="/" baseUrl="" summary="Query Gas Price Floor" %}
{% swagger-description %}
As of the original eth\_gasPrice endpoint.

Query the minimum gas price request for sending transactions via puissant. If the first tx in your puissant has a GasPrice below the floor, your puissant will be rejected instantly.

It barely changes, but this is not a promise.
{% endswagger-description %}

{% swagger-parameter in="header" name="Content-Type" type="String" required="true" %}
`"application/json"`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="id" type="uint64" required="true" %}
Echo. self maintained.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="jsonrpc" type="String" required="true" %}
`"2.0"`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="method" type="String" required="true" %}
`"eth_gasPrice"`
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```javascript
{
    "jsonrpc":"2.0",
    "id":1337,
    "result":"0x37e11d600"
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="post" path="/" baseUrl="" summary="Send Private Transactions" %}
{% swagger-description %}
Send a Raw Transaction in private mode. Subject to [Query Gas Price Floor](puissant-api.md#query-gas-price-floor).

Transaction sent to this RPC will remain inside 48 Club and partner validators without being broadcast, thus will not be packed or only packed by this validator.

Transaction will be handled fully following validation procedure, including but not limit to signature/nonce/gas/gasPrice/balance etc.
{% endswagger-description %}

{% swagger-parameter in="header" name="Content-Type" type="String" required="true" %}
`"application/json"`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="id" type="uint64" required="true" %}
Echo. self maintained.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="jsonrpc" required="true" type="String" %}
`"2.0"`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="method" required="true" type="String" %}
`"eth_sendPrivateRawTransaction"`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="params" required="true" type="[]Tx" %}
Signed transaction (eth_sendRawTransaction style, signed and RLP-encoded)
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Success" %}
```javascript
{
    "id": 1,
    "jsonrpc": "2.0",
    "result": "0xdeadbeef883764809a94a5320e4557102f5a3fdd98dabd8cd48773b0eca00666" // tx hash
}
```
{% endswagger-response %}

{% swagger-response status="200: OK" description="Fail" %}
```javascript
{
    "id": 1,
    "jsonrpc": "2.0",
    "result": "0x" // error for hex
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="post" path="/" baseUrl="" summary="Send Puissant" %}
{% swagger-description %}
If you would like to modify an already-sent puissant, you can send another puissant with the same sender (from addr of the first tx) and raise the gasprice for at least 10%, the previous puissant will be overwritten.
{% endswagger-description %}

{% swagger-parameter in="body" name="id" required="true" type="uint64" %}
Echo. self maintained.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="jsonrpc" type="String" required="true" %}
`"2.0"`
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Content-Type" required="true" %}
`"application/json"`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="method" type="String" required="true" %}
`"eth_sendPuissant"`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="params[0].txs" type="[]Tx" required="true" %}
Signed transactions (eth_sendRawTransaction style, signed and RLP-encoded)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="params[0].maxTimestamp" type="uint64" required="true" %}
puissant valid until timestamp reaches. No more than 2 minutes from now
{% endswagger-parameter %}

{% swagger-parameter in="body" name="params[0].acceptReverting" type="[]TxHash" required="false" %}
The array of hash indicated which transaction(s) are allowed to revert
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Fail" %}
```javascript
{
    "jsonrpc":"2.0",
    "id":1,
    "error":{
        "code":-32000,
        "message":"known sender"
    }
}
```
{% endswagger-response %}

{% swagger-response status="200: OK" description="Success" %}
```javascript
{
    "jsonrpc":"2.0",
    "id":1,
    "result":"a77f8997-0fc1-4d42-94b2-09d4f79c667d"
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="get" path="/api/v1/ping" baseUrl="https://explorer.48.club" summary="query puissant api availability" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-response status="200: OK" description="" %}
```javascript
{
    "message":"pong",
    "status":200
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="get" path="/api/v1/puissant/uuid" baseUrl="https://explorer.48.club" summary="query specific puissant status" %}
{% swagger-description %}
`uuid`

 is what you get from result of 

[Send Puissant](puissant-api.md#send-puissant)


{% endswagger-description %}

{% swagger-response status="200: OK" description="" %}
```javascript
{
    "message":"",
    "status":200,
    "value":{
        "uuid":":uuid",
        "block":"",
        "validator":"",
        "status":"Pending in puissant queue.",
        "info":"OK.",
        "txs":[
            {
                "tx_hash":"tx_hash",
                "status":"innocent noRun",
                "accept_revert":false,
                "created":"2022-08-02T21:43:53+08:00"
            },
            {
                "tx_hash":"tx_hash",
                "status":"innocent noRun",
                "accept_revert":false,
                "created":"2022-08-02T21:43:53+08:00"
            }
        ],
        "created":"2022-08-02T21:43:53+08:00"
    }
}
```
{% endswagger-response %}

{% swagger-response status="404: Not Found" description="" %}
```javascript
{
    "message":"tx not found",
    "status":404
}
```
{% endswagger-response %}

{% swagger-response status="500: Internal Server Error" description="" %}
```javascript
{
    "message":"error info",
    "status":500
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="get" path="/api/v1/score" baseUrl="https://explorer.48.club" summary="query puissant score for sender/ip" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="query" name="address" %}
sender address. if not provided, score of current ip address will be return. 
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```javascript
{
    "message":"ok",
    "status":200,
    "value":{
        "query":"1.1.1.1",
        "score":10,"type":"ip"
    }
}
```
{% endswagger-response %}

{% swagger-response status="500: Internal Server Error" description="" %}
```javascript
{
    "message":"error info",
    "status":500
}
```
{% endswagger-response %}
{% endswagger %}
