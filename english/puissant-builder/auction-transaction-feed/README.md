# Auction Transaction Feed

### Update: <a href="#introduction" id="introduction"></a>

A **filter** was added to the logs and stateDiffs. See [#content-of-logs-and-statediffs](./#content-of-logs-and-statediffs "mention")

**`newPendingAuctions`**: require 48 SP points to subscribe

**`newPendingAuctionsWithState`**: require 710 SP points to subscribe

### Introduction <a href="#introduction" id="introduction"></a>

Introducing Auction Feed, a WebSocket-RPC based subscription data feed that delivers transaction-related data from the 48Club RPC service. Unlike traditional pending transaction subscriptions, Auction Feed focuses specifically on backrun opportunities, providing only the target hashes of these opportunities and the relevant logs and EVM state changes after simulation.

The tracked targets may be individual transactions or bundles, but this distinction is unimportant, as the searcher only needs to assess whether a backrun opportunity exists based on the logs or state changes triggered by the target. When you submit a SendBundle with a target hash, the server automatically prepends the target transaction to the front of your bundle.

### How to Subscribe

Subscribing to Auction Feed push notifications requires no complex third-party packages. You can achieve this using go-ethereum. Simply connect to our WebSocket RPC service and subscribe to either the `newPendingAuctions` or `newPendingAuctionsWithState` event, depending on the data you need.

### How to SendBundle

To submit a bundle that includes a backrun opportunity, use the eth\_sendBundle method with the backrunTarget parameter configured.

* Please note that if the target’s gas price is below 1 gwei, it will not be considered a public pool transaction, so you must include it in your bundle’s average gas price calculation to meet the 1 gwei requirement. However, if the target’s gas price exceeds 1 gwei, you should not factor it into your bundle’s average gas price, as it is treated as a public pool transaction.

### Data Format

Due to the high frequency of updates, we use gzip compression on the raw data to minimize transmission size and enhance delivery speed. This ensures efficient handling of the Auction Feed’s frequent push notifications.

Example code for decompressing gzip data

#### **Subscription to** `newAuctions`

Example code here

This provides essential data to check what a transaction has triggered. It is sufficient for arbitrage strategies, such as those on Uniswap v2 liquidity pools, without requiring simulation.

| Field                  | Format               | Description                                                                                                                  |
| ---------------------- | -------------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| `hash`                 | string (0x hash)     | The auction target ID                                                                                                        |
| `extra_gas_fee`        | uint64               | An additional total gas fee needs to be burned in your bundle to meet the 1 Gwei requirement for including this transaction. |
| `logs`                 | \[] logData          | The logs produced in target simulation                                                                                       |
| exp: logData.`address` | string (0x address)  |                                                                                                                              |
| exp: logData.`topics`  | \[] string (0x hash) |                                                                                                                              |
| exp: logData.`data`    | string (0x data)     |                                                                                                                              |

* Note: if the target gas\_price is below 1 gwei, you should included it into your bundle's average GasPrice calculation. [Check here.](./#how-to-sendbundle)

#### **Subscription to** `newAuctionsWithStates`

Example code here

This provides advanced data for simulation-based strategies. You can create a state from the current moment and apply the state changes provided in the data. Then, begin your simulation as though it were happening in real time on the target transaction.

* You have to sign current unix timestamp string by your 48SoulPoint EOA Example code here
* How to apply state changes on current StateDB Example code here
* `newAuctionsWithStates` has all data in `newAuctions`, plus `states` below

| Field                | Format                         | Description                                       |
| -------------------- | ------------------------------ | ------------------------------------------------- |
| `` `states` ``       | map\[str(1)]map\[str(2)]str(3) | The state key/value changes on all state objects. |
| exp: states.`str(1)` | string (0x address)            | The address of changed state object               |
| exp: states.`str(2)` | string (0x hash)               | changed Key in a changed state object             |
| exp: states.`str(3)` | string (0x hash)               | changed Value of Key                              |

### Content of Logs and StateDiffs

The broadcast logs of a transaction are **filtered** (see the topic list below), and only topics related to **backrunning** will be broadcasted, along with the **stateDiffs** corresponding to this Log.Address.

If you believe we should include additional topics for the **backrunning auction**, please let us know at @infra\_48club.

| Signature                                                                                                                                                                                                            | topics\[0]                                                         |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------ |
| Sync(uint256, uint256)                                                                                                                                                                                               | 0xcf2aa50876cdfbb541206f89af0ee78d44a2abf8d328e37fa4917f982149848a |
| Sync(uint112, uint112)                                                                                                                                                                                               | 0x1c411e9a96e071241c2f21f7726b17ae89e3cab4c78be50e062b03a9fffbbad1 |
| Sync(uint112 reserve0, uint112 reserve1, uint256 totalSupply)                                                                                                                                                        | 0x17be3acebd510daa18778e1ee1fbaf88237b124dc0803c3be2fd4f99f3e69d33 |
| Sync(uint256 vReserve0, uint256 vReserve1, uint256 reserve0, uint256 reserve1)                                                                                                                                       | 0x2f9d55abfefdfd4c3a83e00a1b419b3c2fe4b83100c559f0e2213e57f6e0bba9 |
| Swap(address,address,int256,int256,uint160,uint128,int24)                                                                                                                                                            | 0xc42079f94a6350d7e6235f29174924f928cc2ac818eb64fed8004e115fbcca67 |
| Burn(address,int24,int24,uint128,uint256,uint256)                                                                                                                                                                    | 0x0c396cd989a39f4459b5fa1aed6a9a8dcdbc45908acfd67e028cd568da98982c |
| Mint(address,address,int24,int24,uint128,uint256,uint256)                                                                                                                                                            | 0x7a53080ba414158be7ec69b987b5fb7d07dee101fe85488f0853ae16239d0bde |
| Swap(address,uint256,uint256,uint256,uint256,address)                                                                                                                                                                | 0xd78ad95fa46c994b6551d0da85fc275fe613ce37657fb8d5e3d130840159d822 |
| Swap(index\_topic\_1 address sender, index\_topic\_2 address recipient, int256 amount0, int256 amount1, uint160 sqrtPriceX96, uint128 liquidity, int24 tick, uint128 protocolFeesToken0, uint128 protocolFeesToken1) | 0x19b47279256b2a23a1665c810c8d55a1758940ee09377d4f8d26497a3577dc83 |
| Swap(index\_topic\_1 address sender, uint256 amount0In, uint256 amount1In, uint256 amount0Out, uint256 amount1Out, index\_topic\_2 address to, uint256 feeInPrecision)                                               | 0x606ecd02b3e3b4778f8e97b2e03351de14224efaa5fa64e62200afc9395c2499 |
| Swap(index\_topic\_1 bytes32 id, index\_topic\_2 address sender, int128 amount0, int128 amount1, uint160 sqrtPriceX96, uint128 liquidity, int24 tick, uint24 fee)                                                    | 0x40e9cecb9f5f1f1c5b9c97dec2917b7ee92e57ba5563708daca94dd84ad7112f |
| ModifyLiquidity(index\_topic\_1 bytes32 id, index\_topic\_2 address sender, int24 tickLower, int24 tickUpper, int256 liquidityDelta, bytes32 salt)                                                                   | 0xf208f4912782fd25c7f114ca3723a2d5dd6f3bcc3ac8db5af63baa85f711d5ec |
| fourmeme Create                                                                                                                                                                                                      | 0x396d5e902b675b032348d3d2e9517ee8f0c4a926603fbc075d3d282ff00cad20 |
