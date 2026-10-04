# 48 SoulPoint Benefits

### Limitation by Membership Level

The following documentation describes the access permissions and conditions for users at different levels of 48 SoulPoint membership. Users are classified into several levels, each with different access conditions based on the number of 48SoulPoints ~~held~~ **HODL**.

#### Access Conditions Table

| Membership Level           | RateLimit         | Bundle Limit    | Additional                              |
| -------------------------- | ----------------- | --------------- | --------------------------------------- |
| **Unsigned Guest**         | 50 cond txs / 9s  | 5 txs / bundle  | -                                       |
| **Signed Guest (< 48 SP)** | 50 cond txs / 9s  | 6 txs / bundle  | <p>Failure detail,<br>Websocket RPC</p> |
| **Entry (≥ 48 SP)**        | 100 cond txs / 9s | 10 txs / bundle | <p>Failure detail,<br>Websocket RPC</p> |
| **Gold (≥ 100 SP)**        | 200 cond txs / 9s | 15 txs / bundle | <p>Failure detail,<br>Websocket RPC</p> |
| **Platinum (≥ 480 SP)**    | 500 cond txs / 9s | 50 txs / bundle | <p>Failure detail,<br>Websocket RPC</p> |

#### Explanation of Terms

* **Cond Txs**: Represents the maximum number of ongoing (currently being simulated) and failed transactions permissible within a 9-second time window. This count includes bundle-transactions and private-transaction-services.
* **Failure detail**: When a bundle simulation fails, the RPC provides specific reasons for the failure instead of a generic "bundle execution failed" message. For example, an error could be detailed as: "\[0xc5b2..a2eb] execution reverted due to insufficient funds for gas \* price + value: available balance is 7195, but transaction cost is 7840, resulting in an overshot of 645."
* **Websocket RPC**: Enables the bundles and private transactions sending through a websocket connection.

### Signing Method (For Go, use the code blow)

```go
package types

import (
	"bytes"
	"crypto/ecdsa"
	"github.com/ethereum/go-ethereum/common"
	"github.com/ethereum/go-ethereum/common/hexutil"
	"github.com/ethereum/go-ethereum/crypto"
)

type SendBundleArgs struct {
	Txs                []hexutil.Bytes `json:"txs"`
	MaxBlockNumber     uint64          `json:"maxBlockNumber"`
	MinTimestamp       *uint64         `json:"minTimestamp"`
	MaxTimestamp       *uint64         `json:"maxTimestamp"`
	RevertingTxHashes  []common.Hash   `json:"revertingTxHashes"`
	SoulPointSignature hexutil.Bytes   `json:"48spSign"` // <<<<<<------ This is the 48SP signature
}

// Sign48SPMember signs the hash concatenation of the hashes of the given transactions with the given private key.
// private key is the private key of the soul point member
func Sign48SPMember(prvKey *ecdsa.PrivateKey, txs []*Transaction) (hexutil.Bytes, error) {
	var hashes bytes.Buffer
	hashes.Grow(common.HashLength * len(txs))
	for _, tx := range txs {
		hashes.Write(tx.Hash().Bytes())
	}

	return crypto.Sign(crypto.Keccak256(hashes.Bytes()), prvKey)
}
```

### Signature Result Verification&#x20;

{% hint style="info" %}
For other programming languages, we do not provide code examples. Please use the following example to verify that your signature method is correct.
{% endhint %}

```
tx1 raw hex: 0xf8ad82e69c85012a05f200830329189455d398326f99059ff775485246999027b319795580b844a9059cbb0000000000000000000000006df68f71f19081751850160118fc755bfeb036120000000000000000000000000000000000000000000000056bc75e2d631000008193a0ade014e655a2d1645efb5f37a142c2369ab96d2d962861dd353fa33fe15b76cda041b291fb742d07c0d475cb2611e9dd6010bbd052c79f99e73c7f9e3653afb6f7
tx2 raw hex: 0xf8ec8307f58584b8c6fe72830927c094bddbcbaa9cf9603b7055aad963506ede71692f1280b8830000000300000000000000000000000000000000000000000000000087250559b9145f9000000000000000000000000000000000000000000000011f35cc4934f0b52a80bb4cdb9cbd36b01bd1cbaebf2de08d9173bc095c55d398326f99059ff775485246999027b319795500006400000000000000000af935e000000000000000008193a0f63bed83d054bdecfb6852f980ed62f35922fe5b68296b603b290a3eff41fabea04b73872c7e68f541bc97eba52abf956eac4a0e198b762e1f9a872336f6748eed

tx1 hash: 0x86f69e188bbfcabed6eca4d0be87843d48050ea460677b0b4656fcfb42625e44
tx2 hash: 0xb4062884c020dce933f1c1b7bccd4b94729051f7e4c61caf34ad53a24d06ce5f

the signer address: 0xF3A611849696ec5801745496be3f7C8124C344c3
the signer privateKey: 48acf19375e8a27309fe5394728abc2eb6d5a0a4feb6b6c53207ca1c256a6739

the sign result: 0x83e4b3a6af20e58315554b5bc38a8398cfca44a75d42973a4454378b0dc9cae63c229b52341d1ddfc4e3ad4360e518c1f1363e2d21fcba507e8e2e10e266edd201
```



```
# pip install web3 coincurve
from web3 import Web3
from coincurve import PrivateKey

hex_raw_transaction1 = '0xf8ad82e69c85012a05f200830329189455d398326f99059ff775485246999027b319795580b844a9059cbb0000000000000000000000006df68f71f19081751850160118fc755bfeb036120000000000000000000000000000000000000000000000056bc75e2d631000008193a0ade014e655a2d1645efb5f37a142c2369ab96d2d962861dd353fa33fe15b76cda041b291fb742d07c0d475cb2611e9dd6010bbd052c79f99e73c7f9e3653afb6f7'
hex_raw_transaction2 = '0xf8ec8307f58584b8c6fe72830927c094bddbcbaa9cf9603b7055aad963506ede71692f1280b8830000000300000000000000000000000000000000000000000000000087250559b9145f9000000000000000000000000000000000000000000000011f35cc4934f0b52a80bb4cdb9cbd36b01bd1cbaebf2de08d9173bc095c55d398326f99059ff775485246999027b319795500006400000000000000000af935e000000000000000008193a0f63bed83d054bdecfb6852f980ed62f35922fe5b68296b603b290a3eff41fabea04b73872c7e68f541bc97eba52abf956eac4a0e198b762e1f9a872336f6748eed'
private_key_hex = '48acf19375e8a27309fe5394728abc2eb6d5a0a4feb6b6c53207ca1c256a6739'

tx1_hash = Web3.keccak(Web3.to_bytes(hexstr=hex_raw_transaction1))
tx2_hash = Web3.keccak(Web3.to_bytes(hexstr=hex_raw_transaction2))

print("tx1_hash:", tx1_hash.hex())
print("tx1_hash:", tx2_hash.hex())

private_key = PrivateKey(bytes.fromhex(private_key_hex))
signature = private_key.sign_recoverable(Web3.keccak(tx1_hash + tx2_hash), hasher=None)
print("signature:", signature.hex())
```
