---
description: Designed as Standard RPC Style Endpoint
---

# Puissant API

{% swagger method="post" path="/" baseUrl="" summary="Query Gas Price Floor" %}
{% swagger-description %}
Override of original eth\_gasPrice endpoint.

Query the minimum gas price request for sending transactions via puissant. Transactions sent to puissant with a GasPrice below the floor will be rejected, please make sure your gasPrice (or averaged GasPrice) is equal to or greater than the result.
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

Transaction sent to this RPC will remain inside this validator without being broadcast, thus will not be packed or only packed by this validator.

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

Puissant is natively in private mode.

Txs in a puissant should be ordered following descending gasPrice. Gas price of the first tx in puissant must not be less than [#query-gas-price-floor](puissant-api.md#query-gas-price-floor "mention") otherwise the entire puissant will be **failed instantly**.

All txs in the puissant will be packed in block in the same order unless one of following cases:

**CASE EXPIRED**

When current time  > `maxTimestamp` , the puissant will expire.

**CASE INVALID**

When gasPrice of the first tx in the bundle is less than 21000, the puissant will be considered invalid.

**CASE REVERTED**

When one tx of the puissant is reverted (for any reason) and this tx-hash is not in `acceptReverting` parameter**.**

**CASE BEATEN**

When the puissant contains one tx which is also in another puissant with higher first-tx-gas-price, the other puissant will be served with p**riority.**





Once sealed, those txs with exactly same gasPrice will be packed **consecutively**, but this is **not guaranteed** for the entire puissant.

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

{% swagger-response status="200: OK" description="Success" %}
```javascript
{
    "jsonrpc": "2.0",
    "id": 48,
    "result": null
}
```
{% endswagger-response %}

{% swagger-response status="200: OK" description="Fail" %}
```javascript
{
    "jsonrpc":"2.0",
    "id":48,
    "result": "0x" // error for hexnull
}
```
{% endswagger-response %}
{% endswagger %}
