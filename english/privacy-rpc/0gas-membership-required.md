---
hidden: true
---

# 0Gas (membership required)

As far as：&#x20;

1. Conditions applied  ([Entry Level Member](../48-soul-point/entry-member.md))
2. Use our RPC service.

You can enjoy a gas discount. Please notice there are conditions for this discount. The max allowed gasLimit quota is **`Your Soul Point * 10000`**

If total gasLimit of your discounted txs exceeds the quota within a 252 seconds time-window, your following tx could be rejected.

Specially, if you use `https://0.48.club` for RPC URL, the default gas price returned will be 1 wei because in certain wallets 0 gas price may trigger unexpected bug. Don't worry, 1 wei = 0.000000001 gwei, it's almost 0.

Since not all the validators seal low-gas-price txs (while 48 Club and partners do), gas discounted tx may be sealed a bit slower, it's totally fine.

_\*Please don't exploit the service, we have a blacklist mechanism._



## Quota Remaining api endpoint

```
% curl https://0.48.club -d '{ 
    "jsonrpc": "2.0", 
    "method": "eth_get0GweiGasRemaining", 
    "params": ["0xaACc290a1A4c89F5D7bc29913122F5982916de48"], 
    "id": "0"
    }' -H 'Content-Type: application/json' -s | jq
%

{
  "jsonrpc": "2.0",
  "id": "0",
  "result": 605280000
}

```
