---
hidden: true
---

# 0Gas sponsorship

Stable-coin transferring could be accepted with as low as 0 gwei gas price across **ALL** 48 RPC

Contact rpc@48.club for your stable-coin being listed

## API for Wallets

{% embed url="https://github.com/bnb-chain/BEPs/blob/master/BEPs/BEP-414.md" %}

We implements the `pm_isSponsorable` method 100% compatible with BEP-414, please check api definition in the link above. The general idea is:

1. Query `pm_isSponsorable` endpoint before packing an end-user transaction.
2. Apply 0 gwei to actually pack and broadcast this transaction if the result is true.

## Rules

To be sponsorable ,a tx must meet rules below:

1. It's an interaction to a contract address in following [white list](0gas-sponsorship.md#white-list).
2. Gas limit required is not greater than 300,000.
3. Regarding potential exploition, token amount must not be less than 48e16 if the method called is one of `transfer/transferFrom/increaseAllowance/decreaseAllowance/burn/burnFrom`



## White List

```
USDT 0x55d398326f99059fF775485246999027B3197955
USDC 0x8AC76a51cc950d9822D68b83fE1Ad97B32Cd580d
FDUSD 0xc5f0f7b66764F6ec8C8Dff7BA683102295E16409
LisUSD 0x0782b6d8c4551B9760e74c0545a9bCD90bdc41E5
```
