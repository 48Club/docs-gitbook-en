# Send Bundle

### Introduction <a href="#introduction" id="introduction"></a>

This document details the utilization of the `eth_sendBundle` RPC method, pivotal for submitting transaction bundles within the puissant-builder system, now entering its version 2. This iteration introduces enhanced auction and sorting flexibility compared to the first generation, puissant-mev.

Version 2 of puissant-builder offers a more dynamic auction mechanism for bundle submission. The auction price is composed of two main components: the total gas fees of transactions in the bundle that are not from the tx\_pool and the BNB directly transferred to the Builder Control EOA. The calculation formula accounts for the effects of BEP95, whereby 10% of the gas fees are burnt, thus applying a 10% discount to the gas fee component. Direct transfers to the Builder Control EOA are prioritized in the auction ranking, providing a significant advantage in bundle sorting.

### Builder Control EOA

**Address:** `0x4848489f0b2BEdd788c696e2D79b6b69D7484848`

**Description:** This is the externally owned account (EOA) managed by the builder. It is integral to handling transactions and prioritizing bundle sorting within the puissant-builder system. Strategic transactions directed to this EOA can significantly influence the sorting order, leveraging network efficiencies for optimized transaction execution.

### Bundle Sorting

The bundle sorting algorithm employs the following formula:

$$
[ \left(\text{sum(GasFee of TransactionUnique)} \times 0.9\right) + \left(\text{BNB sent to Builder Control EOA}\right) ]
$$

**TransactionUnique:** Refers to a transaction included in the bundle that does not appear in the public transaction pool.

### Importance and Benefits of [48SoulPoint Member](48-soulpoint-benefits.md) Signature

The inclusion of a 48SoulPoint Member signature with your bundle submission enhances request rate limits and the number of transactions per bundle, along with additional services. It is highly recommended to leverage this feature to maximize the benefits within the puissant-builder system.

### Question?

Join [Telegram Group](https://t.me/infra_48club)

### Request Parameters

The table below outlines the parameters required to submit a bundle:

| Parameter           | Mandatory | Format       | Example             | Description                                                                                                                                                              |
| ------------------- | --------- | ------------ | ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `txs`               | Mandatory | array\[hex]  | \["0x…f1", "0x…b7"] | List of signed raw transactions.                                                                                                                                         |
| `backrunTarget`     | Optional  | hash         |                     | [For Auction Backrun Target Idiantify](auction-transaction-feed/)                                                                                                        |
| `maxBlockNumber`    | Optional  | uint64       | 40140239            | Maximum block number for the bundle's validity. Default is current block number + 100.                                                                                   |
| `maxTimestamp`      | Optional  | uint64       | 1721003319          | Maximum Unix timestamp (in seconds) for the bundle's validity.                                                                                                           |
| `revertingTxHashes` | Optional  | array\[hash] | \["0x…1a", "0x…2b"] | List of transaction hashes allowed for revert.                                                                                                                           |
| `noMerge`           | Optional  | bool         | false               | Bundle Merge is allowed to maximize profit. Default setting increases your chances of winning the auction.                                                               |
| `positionFirst`     | Optional  | bool         | false               | Restrict bundle position strictly based on priorityFee. When disabled (default), late-arriving bundles may be appended to tail to guarantee landing in the current block |
| `48spSign`          | Optional  | hex          | 0xd4cb...6300       | 48SoulPoint Member EOA signature, [check here for the details](48-soulpoint-benefits.md)                                                                                 |

### Request Example <a href="#request-example" id="request-example"></a>

```
{
  "jsonrpc": "2.0",
  "id": "1",
  "method": "eth_sendBundle",
  "params": [
    {
      "txs":["0x…f1", "0x…b7"],    // List of signed raw transactions
      "maxBlockNumber":39177941,   // The maximum block number for the bundle to be valid, with the default set to the current block number + 100
      "maxTimestamp":1710829390,   // Expected maximum Unix timestamp (in unix) for the bundle to be valid
      "revertingTxHashes":[
        "0x2a5b3732b95940abb9f7102d8cd04c35db61b66836d1471a1a01e37168078bf9"
        ],                         // List of transaction hashes allowed for revert
      "48spSign":"0x229252945fffd4ca018d4aa8f1e406a6b23457448c2574499e6e4a83d58d440e316044fe1617a9f10255af037a0d7da91a64e6d6a0ab65eeeb4bf6ced5f3b0f501"
    }
  ]
}‍
```

### Response Example <a href="#response-example" id="response-example"></a>

```
{
 "jsonrpc":"2.0",
 "id":"1",
 "result":"0x274a……053b"  //bundle hash
}‍
```

```
{
  "jsonrpc":"2.0",
  "id":"1",
  "error":{
    "code":-38000,
    "message":"bundle already exist"
    }
}
```
