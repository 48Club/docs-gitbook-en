# BSC Validator

BNB48 is one of several earliest BSC community validators.  [**Stake now**](https://www.bnbchain.org/en/staking/validator/bva1ygrhjdjfyn2ffh5ha5llf5g6l3wxjt29hz9q4s)****

RPC URL:

```
https://rpc-bsc.bnb48.club
wss://rpc-bsc.bnb48.club

https://testnet-rpc-bsc.bnb48.club \\ testnet
```

Mainnet RPC works in privacy mode. Txs sent to mainnet RPC will remain inside BNB48 and partners without being broadcasted, thus will not be packed or only packed by BNB48 and partners.

#### Pros:&#x20;

1. Front-run-resisted because arb-bot can't see your tx in advance of block sealing.
2. Wallet transparency. No programming skill is needed, just fill this RPC URL in your wallet.

#### Cons:&#x20;

1. Since only BNB48 and partners do the sealing for fonce, txs may be sealed a bit slower, it's total normal.
2. Higher gasPrice requirement, [#query-gas-price-floor](enhanced-rpc/puissant-api.md#query-gas-price-floor "mention")
