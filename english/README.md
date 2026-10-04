# F.A.Q.

{% hint style="info" %}
**Where is 48 Club's headquarters?**

48 Club runs completely decentralized.
{% endhint %}

{% hint style="info" %}
**Is 48 Club a whales club?**

Although many whales are members of 48 Club, it's not a mandatory membership qualification.
{% endhint %}

{% hint style="info" %}
**Does 48 Club belong to Binance?**

No.
{% endhint %}

{% hint style="info" %}
**Is 48 Club financially supported by Binance?**

No.
{% endhint %}

{% hint style="info" %}
**error: insufficient gasprice increasement for overwriting ?**

Please refrain from distributing transactions originating from a single address across multiple puissants. The acceptance of a puissant transaction is not determined at the time of your request, and there's a high likelihood of rejection if the nonces are incorrectly arranged.

We won't check all txs because it's too costy. But we do check the first tx of each puissant aka the **sender**.&#x20;

You should avoid sending multiple puissants from a single sender. Otherwise, it will be considered an attempt at replacement, which will require an increase in gas price.
{% endhint %}

{% hint style="info" %}
**My puissant was not entirely landed or not even by 48Club partner validators**

This is expected behavior when chain-reorg occurs, and it is also seen in ETH. Shit happens.

Chain-reorg will make already broadcasted block not valid anymore, thus all txs included turned public. In this situation, the next validator which is likely not a partner validator may not sealed the block as puissant did.

To prevent serious lost, you can add coinbase check, parent hash check, block number check in your contract to revert your tx in this case.
{% endhint %}

{% hint style="info" %}
**error:rlp: expected input list for types.LegacyTx**

It's due to an unmerged type2 compatibility feature after EIP-1599.

Before BSC finally merge from mainstream, you can simply bypass it by using `tx.MarshalBinary` for bytes generating.


{% endhint %}



{% hint style="info" %}
**Is it possible to send transaction with 0 gas ?**

The actual question is is it possible for 48Club to accept tx with 0 gas.

Yes, but with conditions.

1. Our RPC service accept 0 gas transactions with conditions. Check  [Privacy RPC](privacy-rpc/) part.
2. When you are sending a bundle to Puissant, the average gas price instead of representative gas price is checked. Thus you can send txs with 0 gas as far as there are other txs paying enough gas.
{% endhint %}

{% hint style="info" %}
~~**invalid, require descending sort transactions by gas price**~~

~~Pretty sure it's not an problem to sort descending by gas price, the real question is how do we define "gas price".~~

~~Introduced by EIP-1599, we have several new concepts as "base price" "priority price" "max price". Different with Ethereum, "base price" on bsc will keep being 0 with no exception, so "max price" is meaningless on bsc.~~

~~So, if you were dealing with legacy type of tx, everything remains; otherwise, just focus on the "priority price", make sure you are sorting your txs by "priority price" other than "max price".~~
{% endhint %}

{% hint style="info" %}
~~**error: bad score**~~

~~Please check the rules of~~ [~~score~~](deprecated/puissant/score.md)~~. We encourage careful local consideration before actually sending your puissant request.~~

~~It is recommended to monitor your score dynamiclly, when it is reaching down to 0, you can "buy some score" by sending some succeed-for-sure puissant.~~


{% endhint %}

{% hint style="info" %}
~~**Does puissant accept 1 gwei transaction ?**~~

~~Puissant and 1gwei.48.club are two separate services, and therefore, the rules are not entirely the same.~~

~~In puissant service, gas price checks are carried out by each validator locally. It is subject to each validator's respective setting.~~

~~Therefore, the answer is, it depends.~~&#x20;

~~If the final validator openly accepts 1 gwei transactions or even lower gas price, your puissant containing 1 gwei tx will be fine.~~

~~If the final validator requires a higher gas price floor than 1gwei, you can still include 1 gwei tx in your puissant as far as **the sender address of this puissant holds a 48er NFT**, the validator will confirm the NFT balance and accept the&#x6E;**.**~~
{% endhint %}

{% hint style="info" %}
~~When you have a negative score and can't send puissant anymore, here are what you can do:~~

1. ~~Wait till next hour. The score is calculated within a 24-hour window, thus even in worst situation your score will be back to initial value after 24 hours.~~
2. ~~Hold a 48er NFT. Negative score will be ignored if the puissant sender address holds an 48er NFT.~~
{% endhint %}

