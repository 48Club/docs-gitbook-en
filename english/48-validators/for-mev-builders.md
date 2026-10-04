# For MEV Builders

All 48 Validators act together in front of BSC MEV builders.

### Access Control

The Builder Relay is accessible at: `https://builder-relay.48.club`

* Validator nodes are located in Frankfurt, Germany and proxied by Cloudflare.
* Each partner brand may appoint two builders, each submitting up to three bids per block.
* For adding or adjusting builders, please contact the 48Club team via [Telegram](https://t.me/+nhIQNj5G3lY2M2U9).
* Builders who meets expectations `reward does not achieve the expectation` will be banned from the 48Club Builder Relay following manual review.

\
MEV Support



MEV-related functions (adjustments) are as follows:

* `mev_sendBid(arg1: types.`<mark style="color:yellow;">`BidArgsUpgrade`</mark>`)`: Returns `(common.Hash, error)`

```go
type BidArgsUpgrade struct {
    RawBid          *RawBid
    Signature       hexutil.Bytes `json:"signature"`
    PayBidTx        hexutil.Bytes `json:"payBidTx"`
    PayBidTxGasUsed uint64        `json:"payBidTxGasUsed"`

    // 48Club Specific Fields
    NontaxableFee   *big.Int      `json:"nontaxableFee"` // BNB amount exempt from BEP95 taxation, transferred to the 48Club MEV collateral address
}
```

* `mev_listCoinbase()`: Returns `([]common.Address, error)`
* `mev_Params()`: Returns `(types.MevParams, error)`
* ~~`mev_bestBidGasFee`~~ _**obsoleted in order to encourage blind bid**_

### Bidding Preference



Preferences for bid evaluation:

* **Bid Evaluation**: After BEP95 adjustments (10% burnt of the original block reward), bids are evaluated based on net rewards. Contribution is calculated as `bidReward = sumTxGasFee * 0.9 + directTransferBNB`.
* **directTransferBNB**: Direct transfer BNB to the 48Club MEV collateral address: `0x1F732D6eA3BD247F0a400DBbeB479adA6D9Bda8C`.
* **Payment Preferences**: Preference for direct transfers over gas fee burns for builder bribes. Payback transactions are marked but not processed in the final block.
* ~~**Puissant-Builder Priority**: The Puissant-Builder receives a 20% weight increase due to its status as a 48Club brand product.~~   Removed since Feb 11th 2025.



48Club reserves the right to modify the rules at any time and may request special adjustments in the future to better serve the 48Club RPC users.
