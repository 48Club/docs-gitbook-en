# Cash Back

48Club ensures that arbitrage happens within our blockchain blocks, allowing users to receive a **cashback** from MEV (Miner Extractable Value) fees, all while keeping trades safe from sandwich attacks.&#x20;

How It Works: We share a simplified version of each transaction’s details with MEV users. This provides enough information for them to identify arbitrage opportunities without making the transaction vulnerable to front-running or sandwiching. If an arbitrage is found, 48Club collects a fee, part of which is refunded to the user. This process also helps reduce slippage when trading in a single liquidity pool.

## Customized RPC

If you have your own project or wallet portal, it is highly recommended that you set your own cashback RPC from 48Club, upon which it is allowed to set a dedicated cash-back wallet for transactions pass through.

You can also access usage stats of this specific domain.

## Some existing partners:

```
    #---partnership with dApps---
    https://pancake.rpc.48.club # for pancake
    https://four.rpc.48.club # for four.meme
    
    #---partnership with wallets
    https://math.rpc.48.club #for mathwallet
    https://tp.rpc.48.club # for tokenpocket
    https://bitget.rpc.48.club # for bitget wallet
    https://bn.rpc.48.club # for binance wallet
    https://ok.rpc.48.club # for ok wallet
    https://uxuy.rpc.48.club # for uxuy wallet
    https://safepal.rpc.48.club # for safepal wallet
    https://gate.rpc.48.club #for gate wallet
    https://tw.rpc.48.club # for trust wallet
    https://metamask.rpc.48.club # for metamask
    https://bybit.rpc.48.club # for bybit wallet
```

### Statistics

```
func stat_walletCount() int64
func stat_txCount() int64
func stat_walletList() []string
```

```
➜  ~ curl https://bscrpc.pancakeswap.finance \
        -d '{
                "jsonrpc": "2.0",
                "method": "stat_walletCount",
                "params": [],
                "id": 83
            }' \
        -H 'Content-Type: application/json' -s | jq
{
  "id": 83,
  "jsonrpc": "2.0",
  "result": 459
}
➜  ~
```

Contact rpc@48.club for details.
