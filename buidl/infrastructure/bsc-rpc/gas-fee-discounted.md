---
description: 'Puissant: [Fr] Powerful.'
---

# Gas fee discounted

Hold [$KOGE](https://bscscan.com/token/0xe6df05ce8c8301223373cf5b969afcb1498c5528) to enjoy discount of BSC gas fee !&#x20;

As far as：&#x20;

1. There is at least 1 KOGE in your wallet(Including wallet balance, DAO-staked and [48er-nft.md](../../../dao/governance/48er-nft.md "mention")).&#x20;
2. Use this RPC -> `https://1gwei.48.club`

You can enjoy a gas discount.&#x20;

Please notice there are conditions for this discount. The more gasLimit your tx has, the more KOGE you may need to be eligible

Effective KOGE balance includes:

1. $KOGE in your wallet. Only up to 10 will be counted.
2. $KOGE staked to [DAO](https://www.bnb48.club/staking) will be fully counted. $KOGE in 48 Farm will **NOT** be counted.
3. Each held 48er NFT is considered as 1,000,000 KOGE holding.

<pre><code>Effective KOGE or eKOGE
= min(10,Koge Balance) + Koge Staked + 48erNFTBalance * 1,000,000

<strong>eKOGE      max gasLimit for 1gwei discount
</strong>&#x3C;1        0
1~10      480000
100       960000
1000      1920000
10000     3840000
...
//10 times holding double the max gasLimit
</code></pre>

When the gasLimit exceeds your eligible quota, you can&#x20;

1. hold more KOGE or&#x20;
2. set a higher gasPrice.&#x20;

A recommended gasPrice will be included in the error msg, just set the new gasPrice and send your tx again.

Since not all the validators seal low-gas-price txs (while 48 Club and partners do), gas discounted tx may be sealed a bit slower, it's totally fine.

_\*We only accept no more than 1 discounted transaction from identical sender each block, so please hold your tx until the previous one gets confirmed or rejected._

_\*Please don't exploit the service, we have a blacklist mechanism._

_\*For partner project (e.g. Alpaca) there's no restriction on gasLimit._

_\*Discounted RPC work in privacy mode too._

Check [github ](https://github.com/48Club/enhanced\_rpc)for details and issues. Consult _rpc@48.club_ for partnership.
