# Code Example

Data Type

```go
type AuctionDataPkg struct {
	TargetHash  string `json:"hash"`
	ExtraGasFee uint64 `json:"extra_gas_fee"`

	Logs   []*RPCAuctionLogJson         `json:"logs"`
	States map[string]map[string]string `json:"states"`
}

type RPCAuctionLogJson struct {
	Address string   `json:"address"`
	Topics  []string `json:"topics"`
	Data    string   `json:"data"`
}
```



### Subscribe Auctions

method: newPendingAuctions | newPendingAuctionsWithState

```go
func TestSubscribeNewPendingAuctionsWithState(t *testing.T, prv *ecdsa.PrivateKey) {
	client, err := rpc.Dial("wss://puissant-builder.48.club/")
	if err != nil {
		t.Fatal(err)
	}

	nowTS, signSP, err := signCurrentTimestamp(prv)
	if err != nil {
		panic(err)
	}

	ch := make(chan []byte, 10)
	sub, err := client.EthSubscribe(context.Background(), ch, method, nowTS, signSP)
	if err != nil {
		panic(err)
	}

	for {
		select {
		case err := <-sub.Err():
			log.Fatal(err)
		case rawData := <-ch:
			auction, err := decompress(rawData)
			if err != nil {
				panic(err)
			}
			fmt.Println("tx_hash", auction.TargetHash, "gas_used", auction.GasUsed, "gas_price", auction.GasPrice)
			for index, eachLog := range auction.Logs {
				fmt.Println(index, eachLog.Address, eachLog.Topics, eachLog.Data)
			}
			
			// if method == newPendingAuctionsWithState
			count := 0
			for addr, storage := range auction.States {
				for key, value := range storage {
					fmt.Println("storage change", count, addr, key, value)
					count++
				}
			}
			fmt.Println()
		}
	}
}
```

### Utils

```go
func applyStateChanges(state *state.StateDB, changes map[string]map[string]string) {
	for addr, storage := range changes {
		var ch = make(map[common.Hash]common.Hash)
		for key, value := range storage {
			ch[common.HexToHash(key)] = common.HexToHash(value)
		}
		state.SetStates(common.HexToAddress(addr), ch)
	}
	state.Finalise(true) // <<<--- This is important, don't forget
}

func decompress(rawData []byte) (auction *ethapi.AuctionDataPkg, err error) {
	var gz *gzip.Reader
	gz, err = gzip.NewReader(bytes.NewReader(rawData))
	if err != nil {
		return
	}
	defer gz.Close()

	err = jsoniter.NewDecoder(gz).Decode(&auction)
	return
}

func signCurrentTimestamp(prv *ecdsa.PrivateKey) (int64, hexutil.Bytes, error) {
	nowTS := time.Now().Unix()
	sign48, err := crypto.Sign(crypto.Keccak256([]byte(strconv.FormatInt(nowTS, 10))), prv)
	if err != nil {
		return 0, nil, err
	}
	return nowTS, sign48, nil
}
```
