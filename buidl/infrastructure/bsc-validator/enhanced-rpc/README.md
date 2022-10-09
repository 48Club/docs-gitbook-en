---
description: 'Foncé: [Fr] Dark. Puissant: [Fr] Powerful.'
---

# Enhanced RPC

No effort was made to hide user footprint.&#x20;

<details>

<summary>Private transaction For end-user</summary>

Mainnet: `https://fonce-bsc.bnb48.club`

Testnet: `https://testnet-fonce-bsc.bnb48.club`

Txs sent to this RPC will remain inside BNB48 validator without being broadcast, thus will not be packed or only packed by BNB48.

#### Pros:&#x20;

1. Front-run-resisted because arb-bot can't see your tx in advance of block sealing.
2. Wallet transparency. No programming skill is needed, just fill this RPC URL in your wallet.

#### Cons:&#x20;

1. Mighty slow confirmation.
2. Higher gasPrice requirement, currently 15gwei.

</details>

<details>

<summary>Enhanced endpoint For professional programmer</summary>

`https://puissant-bsc.bnb48.club //mainnet`

`https://testnet-puissant-bsc.bnb48.club // testnet`

Group txs supported while not breaking gasPrice order priority

Powerful tool for programmers only, you can't use this URL as an ordinary wallet RPC.

Check [puissant-api.md](puissant-api.md "mention")for details.

</details>
