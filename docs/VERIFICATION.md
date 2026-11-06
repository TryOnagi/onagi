# Verification

Every reward, conversion and burn has on-chain evidence. Transaction hashes let anyone check the money independently of the totals shown on the site.

## Public data

| Source | Link |
|---|---|
| DefiLlama | https://defillama.com/protocol/onagi |
| Dune | https://dune.com/onagi/onagi |
| Rewards record | https://onagi.si/payouts |
| Cycle ledger (JSON) | https://onagi.si/data/cycles.json |

The cycle ledger is written only after a cycle has completed on-chain. Its fields (threshold, eligibility, split, totals, per-cycle amounts and transaction hashes) are what the site's own record page reads.

## Public wallets (Robinhood Chain)

| Wallet | Address | Role |
|---|---|---|
| Treasury | `0x0493c28d90f3f0e9353a82c54235bd84c8b66704` | Receives platform fees from the fee vault and claimed creator fees, converts the holder allocation and sends USDG rewards |
| Treasury, cold | `0xF9DDb1e779fcd84c930C1462Cce71d070778E5Ab` | Long-term wallet and creator of the ONAGI token. Key kept offline. Owns the fee vault |
| Fee vault | `0xe2f197bA0D1C0864b86FA36186c0c80B57f54F49` | Contract. Trading fees land here first; the treasury can draw at most $10,000 a day, only into itself |
| Buyback | `0xFb3d7B64d1C99eF19Ce7964220F1C387CF7BEc44` | Receives the buyback allocation in ETH, buys ONAGI and sends it to the burn address |
| Transfer | `0xcDFe1f5A475cC65830e24753E03d705de5dBD971` | Pays network fees for users without ETH and is paid back in USDG |

None of these wallets ever counts as a holder.

## What to check, and how

| Claim | Where to look | What confirms it |
|---|---|---|
| A reward reached a wallet | Reward history, then the explorer | A USDG transfer from the treasury to that address with the listed hash |
| The split was applied | Incoming transfers of the buyback and cold wallets | ETH transfers from the treasury in each cycle: a quarter of the pool to each |
| Holder rewards were converted | The cycle's `convertSig` | An ETH to USDG swap from the treasury |
| Bought tokens were burned | Outgoing transfers of the buyback wallet | ONAGI transfers to the burn address, never to any other address |
| The parameters are the ones in use | `/data/cycles.json` | Threshold, eligibility and split fields match the docs |

---

[Back to README](../README.md) · Full reference: [onagi.si/docs](https://onagi.si/docs)
