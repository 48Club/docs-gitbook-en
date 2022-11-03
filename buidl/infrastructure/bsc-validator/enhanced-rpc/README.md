---
description: 'Foncé: [Fr] Dark. Puissant: [Fr] Powerful.'
---

# Enhanced RPC

Following enhanced RPCs were implemented for extra functionalities, partner validators other than BNB48 alone also serve.

Check [github ](https://github.com/BNB48Club/enhanced\_rpc)for details and issues. Consult _rpc@bnb48.club_ for partnership.

Notice no effort was made to hide user footprint in these services.&#x20;

<details>

<summary>koge-rpc-bsc, gas discount for KOGE holder</summary>

Hold [$KOGE](https://bscscan.com/token/0xe6df05ce8c8301223373cf5b969afcb1498c5528) to enjoy discount of BSC gas fee !&#x20;

As far as：&#x20;

1. There is [48er-nft.md](../../../../dao/governance/voting/48er-nft.md "mention") or at least 1 KOGE in your wallet.&#x20;
2. Use this RPC -> [https://koge-rpc-bsc.bnb48.club](https://t.co/5859ob3MhI)

You can enjoy a gas discount up to 80% then.&#x20;

Please notice there are conditions for this discount. The more gasLimit your tx has, the more KOGE you need to be eligible (unless you hold a 48er NFT, that's the silver bullet).

When the gasLimit exceeds your eligible quota, you can hold more KOGE or set a higher gasPrice. A recommended gasPrice will be included in the error msg, just set the new gasPrice and send your tx again.

Since not all the validators seal txs below 5gwei (while BNB48 and partners do), gas discounted tx may be sealed a bit slower, it's totally normal.

</details>

<details>

<summary>fonce-bsc, private transaction For end-user</summary>

Mainnet: `https://fonce-bsc.bnb48.club`

Testnet: `https://testnet-fonce-bsc.bnb48.club`

Txs sent to this RPC will remain inside BNB48 and partners without being broadcasted, thus will not be packed or only packed by BNB48 and partners.

#### Pros:&#x20;

1. Front-run-resisted because arb-bot can't see your tx in advance of block sealing.
2. Wallet transparency. No programming skill is needed, just fill this RPC URL in your wallet.

#### Cons:&#x20;

1. Since only BNB48 and partners do the sealing for fonce, txs may be sealed a bit slower, it's total normal.
2. Higher gasPrice requirement, [#query-gas-price-floor](puissant-api.md#query-gas-price-floor "mention")

</details>

<details>

<summary>puissant-bsc, grouped txs for professional programmer</summary>

`https://puissant-bsc.bnb48.club //mainnet`

`https://testnet-puissant-bsc.bnb48.club // testnet`

Puissant is a service where grouped txs are supported as an atomic operation without breaking gasPrice based ordering. Puissant is natively in private mode.

Powerful tool for programmers only, you can't use this URL as an ordinary wallet RPC.

Check [puissant-api.md](puissant-api.md "mention")for details.

</details>
