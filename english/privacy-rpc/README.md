---
description: >-
  Based upon 48 infrastructure, we provide following privacy RPC service, as
  well as some more wonderful features.
---

# Privacy RPC

## Access

If you want your own partnership rpc, contact rpc(a)48.club

```
Network URL:
    https://rpc.48.club
Chain ID:
    56
Currency symbol:
    BNB
Block explorer URL:
    https://bscscan.com
```

## Why should one use a Privacy RPC?

In short, keep from sandwich attach, or being front-run.

## How does it work?

As the biggest MEV builder on BSC, 48Club is able to put your transaction into a produced block without broadcasting it publicly, so that front-runner will not be able to see your transaction before its landing.

Using our privacy RPC ensures your transactions are sent to Puissant builder's private mem-pool, mitigating the risk of being sandwiched by attackers.

## Why should one use 48's Privacy RPC?

As the leading BSC builder service provider, we produces most blocks on BSC, which means better chance and speed to pack your transaction a.s.a.p.

Statistics are available at each index 0 transaction of a puissant-proposed block like following:

[https://bscscan.com/tx/0x25b8ae3fb5c0682411be92071afa7ef10c9fd43a061808794ca82041aede0c8e](https://bscscan.com/tx/0x25b8ae3fb5c0682411be92071afa7ef10c9fd43a061808794ca82041aede0c8e)

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

## How do I make sure I am using 48's privacy RPC ?

Check following page

{% embed url="https://pancakeswap.finance/mev" %}

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption><p>You will see this if you are protected by 48Club</p></figcaption></figure>



## How do I make sure my clients are using 48's privacy RPC ?

All RPCs provided by 48Club will respond 0x30, i.e. 48,  to a virtual contract address, 0x48, in the following manner. Since this address does not actually exist, other RPCs will return error messages. This allows the dApp frontend to determine whether the user has enabled 48Club’s RPC in their wallet.

```
➜  ~ curl https://bscrpc.pancakeswap.finance \
        -d '{
                "jsonrpc":"2.0",
                "method":"eth_call",
                "params":[{
                        "to":"0x0000000000000000000000000000000000000048",
                        "value":"0x30"
                        }],
                "id":83}' \
        -H 'Content-Type: application/json' -s | jq
{
  "id": 83,
  "jsonrpc": "2.0",
  "result": "0x30"
}

```
