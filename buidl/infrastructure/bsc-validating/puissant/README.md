---
description: MEV solution on BNB Smart Chain
---

# Puissant

**Puissant** is a permissionless, transparent, and fair ecosystem for efficient MEV extraction and frontrunning protection which preserves the ideals of BNB Smart Chain. Puissant provides a private communication channel between BNB Smart Chain users and validators for efficiently communicating preferred transaction order within a block.

## Why Puissant ?

In 2022, a spike in BNB Smart Chain usage has revealed a set of negative externalities brought by arb bots, as it did on Ethereum, including network congestion (i.e. p2p network load) and chain congestion (i.e. chain-reorg, empty block) caused by inefficient communication between PGA bot operators and validators for transaction order preference.

Resources are wasted, chain user experience is getting worse and meanwhile BNB holders have no chance to enjoy mev profit.

48Club decided to provide this solution. It is inspired by flashbots but improved according to BNB Smart Chain's own community philosophy.&#x20;

What's most important:&#x20;

<mark style="background-color:orange;">**All MEV profit made by Puissant goes to delegators of 48Club and its partner validators.**</mark>

## Timeline

Trial version release

First external partner

Open-Source

## How does it work?

Basically, **Puissant** is a service that completes the special ordering of transactions by accepting off-chain requests, while we use the term **puissant** referring to the concept of **an ordered group of transactions**.

1. On start of each block, puissant starts to listen on new puissant requests.
2. All transactions in puissant should pass basic checking as it is like in an ordinary RPC service, including nonce, gas price, balance etc. Otherwise the entire puissant is considered as **invalid**.
3. The very first transaction in each puissant, aka **the bid**, must have a gas price not less than [the min request](puissant-api.md#query-gas-price-floor), and an actual consumed gas not less than 21000. Also **invalid** if not met.
4. Txs in a puissant must be ordered nonce ascending, gas price descending and then however as you wish. Breaking nonce order or gas price order will also be considered as **invalid**.
5. Valid puissants will be forwarded to partner validators through private link and enter **waitinglist**.&#x20;
6. Once successfully accepted, these transactions will be ordered in the block exactly like how they were in the puissant. Although, those transactions with exactly same gasPrice will be placed **consecutively**, but this is **not guaranteed** for the entire puissant.
7. In default, if any transaction in a puissant gets reverted during block simulation, the entire puissant will be dropped. If it's expected and acceptable result, you need to specify in the request.&#x20;
8. If multiple puissants contain an identical transaction, these puissants are in conflict with each other, and only one of them can be included in the final block. In this scenario, the puissant with the highest bid will receive the highest priority. If, for any reason, the top puissant is removed from consideration, then the next puissant in line will be given top priority. Otherwise, the other conflicting puissants will be excluded at the end of this block period.
9. Puissants remain in the waitinglist, unless get packed or be dropped for one of following reasons :

**CASE EXPIRED**

When current time > `maxTimestamp` , or any tx in the puissant was included in previous block, the puissant expires.&#x20;

**CASE INVALID**

Failed in prechecking.

**CASE REVERTED**

When one tx of the puissant is reverted (for any reason) and this tx-hash is not in `acceptReverting` parameter.

**CASE BEATEN**

When conflicted with other puissant and failed in bidding.
