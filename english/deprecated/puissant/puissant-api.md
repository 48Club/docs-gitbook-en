---
description: Designed as Standard RPC Style Endpoint
---

# API

Access URL

{% hint style="info" %}
`https://puissant-bsc.48.club`

`wss://puissant-bsc.48.club/ws/` (for 48er NFT holders only)
{% endhint %}

## Query Gas Price Floor

<mark style="color:green;">`POST`</mark> `/`

As of the original eth\_gasPrice endpoint.

Query the minimum gas price request for sending transactions via puissant. If the first tx in your puissant has a GasPrice below the floor, your puissant will be rejected instantly.

It barely changes, but this is not a promise.

#### Headers

| Name                                           | Type   | Description          |
| ---------------------------------------------- | ------ | -------------------- |
| Content-Type<mark style="color:red;">\*</mark> | String | `"application/json"` |

#### Request Body

| Name                                      | Type   | Description            |
| ----------------------------------------- | ------ | ---------------------- |
| id<mark style="color:red;">\*</mark>      | uint64 | Echo. self maintained. |
| jsonrpc<mark style="color:red;">\*</mark> | String | `"2.0"`                |
| method<mark style="color:red;">\*</mark>  | String | `"eth_gasPrice"`       |

{% tabs %}
{% tab title="200: OK " %}
```javascript
{
    "jsonrpc":"2.0",
    "id":1337,
    "result":"0x37e11d600"
}
```
{% endtab %}
{% endtabs %}

## Send Private Transactions

<mark style="color:green;">`POST`</mark> `/`

Send a Raw Transaction in private mode. Subject to [Query Gas Price Floor](puissant-api.md#query-gas-price-floor).

Transaction sent to this RPC will remain inside 48 Club and partner validators without being broadcast, thus will not be packed or only packed by this validator.

Transaction will be handled fully following validation procedure, including but not limit to signature/nonce/gas/gasPrice/balance etc.

#### Headers

| Name                                           | Type   | Description          |
| ---------------------------------------------- | ------ | -------------------- |
| Content-Type<mark style="color:red;">\*</mark> | String | `"application/json"` |

#### Request Body

| Name                                      | Type   | Description                                                                |
| ----------------------------------------- | ------ | -------------------------------------------------------------------------- |
| id<mark style="color:red;">\*</mark>      | uint64 | Echo. self maintained.                                                     |
| jsonrpc<mark style="color:red;">\*</mark> | String | `"2.0"`                                                                    |
| method<mark style="color:red;">\*</mark>  | String | `"eth_sendPrivateRawTransaction"`                                          |
| params<mark style="color:red;">\*</mark>  | \[]Tx  | Signed transaction (eth\_sendRawTransaction style, signed and RLP-encoded) |

{% tabs %}
{% tab title="200: OK Success" %}
```javascript
{
    "id": 1,
    "jsonrpc": "2.0",
    "result": "0xdeadbeef883764809a94a5320e4557102f5a3fdd98dabd8cd48773b0eca00666" // tx hash
}
```
{% endtab %}

{% tab title="200: OK Fail" %}
```javascript
{
    "id": 1,
    "jsonrpc": "2.0",
    "result": "0x" // error for hex
}
```
{% endtab %}
{% endtabs %}

## Send Puissant

<mark style="color:green;">`POST`</mark> `/`

If you would like to modify an already-sent puissant, you can send another puissant with the same sender (from addr of the first tx) and raise the gasprice for at least 10%, the previous puissant will be overwritten.

#### Headers

| Name                                           | Type   | Description          |
| ---------------------------------------------- | ------ | -------------------- |
| Content-Type<mark style="color:red;">\*</mark> | String | `"application/json"` |

#### Request Body

| Name                                                      | Type      | Description                                                                 |
| --------------------------------------------------------- | --------- | --------------------------------------------------------------------------- |
| id<mark style="color:red;">\*</mark>                      | uint64    | Echo. self maintained.                                                      |
| jsonrpc<mark style="color:red;">\*</mark>                 | String    | `"2.0"`                                                                     |
| method<mark style="color:red;">\*</mark>                  | String    | `"eth_sendPuissant"`                                                        |
| params\[0].txs<mark style="color:red;">\*</mark>          | \[]Tx     | Signed transactions (eth\_sendRawTransaction style, signed and RLP-encoded) |
| params\[0].maxTimestamp<mark style="color:red;">\*</mark> | uint64    | puissant valid until timestamp reaches. No more than 2 minutes from now     |
| params\[0].acceptReverting                                | \[]TxHash | The array of hash indicated which transaction(s) are allowed to revert      |

{% tabs %}
{% tab title="200: OK Fail" %}
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
{% endtab %}

{% tab title="200: OK Success" %}
```javascript
{
    "jsonrpc":"2.0",
    "id":1,
    "result":"a77f8997-0fc1-4d42-94b2-09d4f79c667d"
}
```
{% endtab %}
{% endtabs %}

## query puissant api availability

<mark style="color:blue;">`GET`</mark> `https://explorer.48.club/api/v1/ping`

{% tabs %}
{% tab title="200: OK " %}
```javascript
{
    "message":"pong",
    "status":200
}
```
{% endtab %}
{% endtabs %}

## query specific puissant status

<mark style="color:blue;">`GET`</mark> `https://explorer.48.club/api/v1/puissant/uuid`

`uuid` is what you get from result of [Send Puissant](puissant-api.md#send-puissant)

{% tabs %}
{% tab title="200: OK " %}
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
{% endtab %}

{% tab title="404: Not Found " %}
```javascript
{
    "message":"tx not found",
    "status":404
}
```
{% endtab %}

{% tab title="500: Internal Server Error " %}
```javascript
{
    "message":"error info",
    "status":500
}
```
{% endtab %}
{% endtabs %}

## query puissant score for sender/ip

<mark style="color:blue;">`GET`</mark> `https://explorer.48.club/api/v1/score`

#### Query Parameters

| Name    | Type   | Description                                                                   |
| ------- | ------ | ----------------------------------------------------------------------------- |
| address | String | sender address. if not provided, score of current ip address will be return.  |

{% tabs %}
{% tab title="200: OK " %}
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
{% endtab %}

{% tab title="500: Internal Server Error " %}
```javascript
{
    "message":"error info",
    "status":500
}
```
{% endtab %}
{% endtabs %}
