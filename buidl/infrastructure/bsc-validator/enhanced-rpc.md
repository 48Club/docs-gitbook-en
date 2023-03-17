---
description: 'Puissant: [Fr] Powerful.'
---

# Gas fee discounted

Discounted RPC work in privacy mode too.

Check [github ](https://github.com/BNB48Club/enhanced\_rpc)for details and issues. Consult _rpc@48.club_ for partnership.

<details>

<summary>koge-rpc-bsc, gas discount for KOGE holder</summary>

Hold [$KOGE](https://bscscan.com/token/0xe6df05ce8c8301223373cf5b969afcb1498c5528) to enjoy discount of BSC gas fee !&#x20;

As far as：&#x20;

1. There is at least 1 KOGE in your wallet(Including wallet balance, DAO-staked and [48er-nft.md](../../../dao/governance/48er-nft.md "mention")).&#x20;
2. Use this RPC -> `https://koge-rpc-bsc.48.club`

You can enjoy a gas discount up to 80% then.&#x20;

Please notice there are conditions for this discount. The more gasLimit your tx has, the more KOGE you need to be eligible

Effective KOGE balance includes:

1. $KOGE in your wallet, up to 10.
2. $KOGE staked to [DAO](https://www.bnb48.club/staking).
3. Each held 48er NFT is considered as 1,000,000 KOGE holding.

<pre><code>Effective KOGE or eKOGE
= min(10,Koge Balance) + Koge Staked + 48erNFTBalance * 1,000,000

<strong>eKOGE      max gasLimit for 1gwei discount
</strong>&#x3C;1        0
1~10      240000
100       480000
1000      960000
10000     1920000
...
//10 times holding double the max gasLimit
</code></pre>

When the gasLimit exceeds your eligible quota, you can hold more KOGE or set a higher gasPrice. A recommended gasPrice will be included in the error msg, just set the new gasPrice and send your tx again.

Since not all the validators seal txs below 5gwei (while 48 Club and partners do), gas discounted tx may be sealed a bit slower, it's totally normal.

\*We only accept no more than 1 discounted transaction from identical sender each block, so please hold your tx until the previous one gets confirmed or rejected.

\*Please don't exploit the service, we have a blacklist mechanism.

\*For partner project (e.g. Alpaca) there's no restriction on gasLimit.

</details>
