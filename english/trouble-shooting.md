# Trouble Shooting

## Uptime Statistics

[https://stats.48.club/](https://stats.48.club/)

## Error Codes

| Code | Summary                              | Description                                                                                                     |
| ---- | ------------------------------------ | --------------------------------------------------------------------------------------------------------------- |
| 4851 | SendBundle reply code violation 4851 | The bundle carries tx that transfer BNB to other builders                                                       |
| 4802 | koge-holder-policy                   | 0 gwei (below 1gwei) gaslimit exceeded, check `eth_get0GweiGasRemaining` upon 0.48.club to find your quota left |
| 429  | too many requests                    | general http ratelimit exceeded, check \`X-Ratelimit-Remaining' in http response header                         |
