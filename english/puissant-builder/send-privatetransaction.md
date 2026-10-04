# Send PrivateTransaction

### Introduction

This group of RPC methods allows users to submit private transactions to the BNB chain. These methods are specifically designed to keep transactions confidential, shielding them from the standard transaction pool to prevent front-running attacks and granting exclusive benefits to members of the 48club with [SoulPointMembership](../48-soul-point/).

### RPC Method Names and Parameters

1. **`eth_sendPrivateTransaction` | `eth_sendRawTransaction`**
   * **Parameters:**
     * `signedRawTransaction (hexutil.Bytes)`
2. **`eth_sendPrivateTransactionWith48SP`**
   * **Parameters:**
     * `signedRawTransaction (hexutil.Bytes)`
     * `48SoulPointMemberSignature (hexutil.Bytes)`
   * **Note:** This method returns the hash of the transaction, similar to the regular `eth_sendPrivateTransaction`.
3. **`eth_sendBatchPrivateTransactionsWith48SP`**
   * **Parameters:**
     * `listSignedRawTransaction ([]hexutil.Bytes)`
     * `48SoulPointMemberSignature (hexutil.Bytes)`
   * **Note:** Unlike the single transaction method, this method does not return individual transaction hashes; it only confirms the request was received without errors or returns an error if the submission fails.

For details on obtaining a `48SoulPointMemberSignature`, [please refer to here](48-soulpoint-benefits.md#signing-method-for-go-use-the-code-blow).

### Response Examples

**Successful Transaction Submission**

For a successful single transaction submission, the response includes the transaction hash:

```json
{
  "jsonrpc": "2.0",
  "id": "1",
  "result": "0x2a5b3732b95940abb9f7102d8cd04c35db61b66836d1471a1a01e37168078bf9"  // Transaction hash
}
```

**Error in Transaction Submission**

If there is an error during submission, such as a mismatch in the required gas price, the error will detail the specific issue:

```json
{
  "jsonrpc": "2.0",
  "id": "1",
  "error": {
    "code": -32000,
    "message": "[private transaction service] require GasPrice=%d, Provide=%d"
  }
}
```
