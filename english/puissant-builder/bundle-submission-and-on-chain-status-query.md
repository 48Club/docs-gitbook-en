---
description: >-
  This API provides a means to query the status of bundle submissions and their
  confirmation on the blockchain, helping users understand if their bundles have
  been submitted and confirmed.
---

# Bundle Submission and On-Chain Status Query

### Endpoint <a href="#introduction" id="introduction"></a>

```
GET https://explore.48.club/v2/bundle?hash=0x000
```

### Request Example <a href="#request-example" id="request-example"></a>

```
curl https://explore.48.club/v2/bundle?hash=0x77818cb1d4f40301d68bb53a66d7df4fba2fe730317309351ce0c198c248c70a
```

### Reponse Example <a href="#reponse-example" id="reponse-example"></a>

```
{
  "submitted": true,
  "confirmed": true,
  "block": 42034652,
  "priority": "3442155127869288",
  "48sp_addr": "0x19029012901290902112acacacacaac901290F",
  "48sp": 520,
  "txs": [
    {
      "tx_hash": "0x3f0452dd05f8ee23b1fa61a735760561ff19ff9b615fd1718440f0fd74c33948",
      "revertible": false
    },
    {
      "tx_hash": "0xde7af1842420a75e717f5f8a33f0f593a94e311062962c354b65abf3935112ce",
      "revertible": false
    }
  ]
}
```

### **Detailed Field Descriptions**

* **`submitted`**:
  * Indicates whether the builder has attempted to submit the bundle for block bidding.
  * `true`: The builder has attempted to submit the bundle to validators.
  * `false`: The builder has not submitted the bundle, possible reasons include:
    1. The query was made too soon, and the builder has not yet reached the submission time.
    2. Any transaction within the bundle was confirmed in a block where we do not have the rights to propose blocks, causing the bundle to become invalid.
    3. The bundle is competing with other bundles and its priority value is lower than others, hence it was not carried for submission.
* **`confirmed`**:
  * Indicates whether the bundle has been confirmed on the blockchain.
  * `true`: The bundle has been successfully included on the blockchain, with the `block` parameter providing the block number.
  * `false`: The bundle has not yet been confirmed on the blockchain, and the `block` parameter will not be present.
* **`block`**:
  * Provides the block number where the bundle was included. This is only provided if `confirmed` is `true`.
* **`priority(optional)`**:
  * A calculated value representing the bundle's bidding priority for block inclusion. It is derived from the sum of all included transaction gas fees adjusted by certain factors and additional direct transfers.
* **`48sp_addr(optional)`**:
  * the 48SoulPoints sign EOA address . ignore if the bundle is unsigned.
* **`48sp(optional)`**:
  * number of 48SoulPoints held. ignore if the bundle is unsigned.
* **`txs`**:
  * A list of transactions within the bundle, each described by:
    * **`tx_hash`**: The hash identifier for the transaction.
    * **`revertible`**: Indicates whether the transaction can be reverted, with `false` signifying it cannot be reverted.

### Additional Information

* **Data Retention**: The API retains records for up to 7 days. Records older than this duration are cleared and cannot be queried.
* **Delayed Response**: Due to blockchain reorganization (reorg) concerns, there is a deliberate delay in updating the `confirmed` status, requiring a wait of five blocks to ensure accuracy.
* **Immediate Status**: If immediate knowledge of the submission status is necessary, this API may not be suitable due to its delayed nature.
* **Error Handling**: If a queried hash does not exist, the API will return an HTTP 404 error code.
* **Rate Limiting**: The API implements rate limiting to prevent excessive use. Please refrain from unnecessary or excessive requests to ensure availability for all users.
