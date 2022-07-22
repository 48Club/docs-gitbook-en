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

{% swagger method="post" path="/" baseUrl="" summary="Send Puissant Bundle" %}
{% swagger-description %}
Send multiple Transactions in a bundle, natively in private mode.

Basically, Txs will still be ordered strictly following descending gasPrice.

A Bundle is a group of txs (must be provided in gasPrice descending order) which will be packed in block strictly in the same order. Those tx in a bundle with exactly same gasPrice will be packed consecutively, but this is **not guaranteed** for the entire bundle

Each Tx will be handled fully following validation procedure, including but not limit to signature/nonce/gas/gasPrice etc.

Instead of separately gasPrice checking, the average gasPrice must not be less than gas price floor otherwise the entire bundle will be rejected.

$$average\ gasPrice = \frac{\sum(gasPrice_i \times e\_gasLimit_i)}{\sum(e\_gasLimit_i)}$$

where

The trust-relay bundle has the higher priority than the normal bundle.

$$\begin{equation} e\_gasLimit_i= \left\{  \begin{aligned} gasLimit_i\qquad if\quad gasPrice_i \le gasPriceFloor   \\ 21000\qquad if\quad gasPrice_i \gt gasPriceFloor   \\ \end{aligned} \right. \end{equation}$$

If two different bundles conflict, the one with higher average gasPrice will be served.
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
bundle valid until timestamp reaches. No more than 2 minutes from now
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
