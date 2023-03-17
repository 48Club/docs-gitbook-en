---
description: Designed as Standard RPC Style Endpoint
---

# Puissant API

Base url if not specified: `https://puissant-bsc.48.club`

{% swagger method="post" path="/" baseUrl="" summary="Query Gas Price Floor" %}
{% swagger-description %}
As of the original eth\_gasPrice endpoint.

Query the minimum gas price request for sending transactions via puissant. If the first tx in your puissant has a GasPrice below the floor, your puissant will be rejected instantly.

It barely change, but this is not a promise.
{% endswagger-description %}

{% swagger-parameter in="header" name="Content-Type" type="String" required="true" %}
`"application/json"`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="id" type="uint64" required="true" %}
`73`
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
Send a Raw Transaction in private mode.

Transaction sent to this RPC will remain inside 48 Club and partner validators without being broadcast, thus will not be packed or only packed by this validator.

Transaction will be handled fully following validation procedure, including but not limit to signature/nonce/gas/gasPrice/balance etc.
{% endswagger-description %}

{% swagger-parameter in="header" name="Content-Type" type="String" required="true" %}
`"application/json"`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="id" type="uint64" required="true" %}
1
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
Send multiple Transactions in a group, called a puissant.

Txs in a puissant should be ordered following descending gasPrice. Gas price of the first tx in puissant must not be less than [#query-gas-price-floor](puissant-api.md#query-gas-price-floor "mention") and gas used of this tx must not be less than 21000, otherwise the entire puissant will be **failed instantly**.

All txs in the puissant will be packed in the next block sealed by our validators in the same order, unless in one of following cases no txs will be packed:

**CASE EXPIRED**

When current time > `maxTimestamp` , or any tx in the puissant was included in previous block, the puissant expires.

**CASE INVALID**

When gasUsed of the first tx in the bundle is less than 21000, the puissant will be considered invalid.

**CASE REVERTED**

When one tx of the puissant is reverted (for any reason) and this tx-hash is not in `acceptReverting` parameter\*\*.\*\*

**CASE BEATEN**

When the puissant contains one tx which is also in another puissant with higher first-tx-gas-price, the other puissant will be served with priority\*\*.\*\*

Once packed, those txs with exactly same gasPrice will be placed **consecutively**, but this is **not guaranteed** for the entire puissant.

If multiple first-tx-of-puissant share one identical sender, it will be considered as spamming the puissant service, all these puissants will be ignored except one.
{% endswagger-description %}

{% swagger-parameter in="body" name="id" required="true" type="uint64" %}
48
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

{% swagger method="get" path="/api/v1/puissant/:uuid" baseUrl="https://explorer.48.club" summary="query specific puissant status" %}
{% swagger-description %}

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
