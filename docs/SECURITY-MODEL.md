# Security model

## Custody

Onagi does not hold user funds. Sign-in creates an embedded wallet for each user. The wallet is the user's own; its key is secured by Privy, not by Onagi. The user grants Onagi a limited permission to trade through three pockets, all in the user's name.

| Pocket | Where it lives | Notes |
|---|---|---|
| **Cash** | USDG in the user's embedded wallet on Robinhood Chain | Token swaps spend from here and rewards arrive here |
| **Perps** | The user's own Lighter account | New positions use isolated margin |
| **Predict** | The user's own Polymarket wallet | Prediction tickets spend from here |

## The trading permission

The same limited signing permission applies to every user.

| Operation | Allowed | Detail |
|---|---|---|
| Open and close positions in the user's name | Yes | Tokens from the wallet, perps from the Lighter account, predictions from the Polymarket wallet |
| Move money between the user's own pockets | On request | Only between accounts in the user's name |
| Pay gas for a move or a swap | Yes | No ETH needed: Onagi supplies the gas and collects it back in USDG as a network fee |
| Send funds to any other address | **No** | Onagi has no feature that does this. Sending to another address is the user's own action, confirmed in the app and signed by the user's own wallet |
| Let the agent act on the user's instructions | Only while its switch is on | Off, the agent only reads and explains |
| Revoke | Any time | Switch the agent off or remove the trading permission on the Agent page |

Removing the permission stops new trades and cancels waiting orders. Open perps positions keep their stop and target and can still be closed; Onagi's trading key for the user's Lighter account is deleted once they are closed.

## Safeguards

- **Isolated margin.** Each new perps position has its own margin and liquidation price, so its loss is limited to that margin.
- **No duplicate sends.** If the result of a send is not yet clear, Onagi checks it on the network and never sends the same transfer twice. Sending the same amount to the same address again within 10 minutes asks for a second confirmation.
- **Fee vault limit.** Trading fees land in a fee vault contract. The treasury can draw at most $10,000 a day from it, only into itself; changing that limit, pausing it or returning funds needs the offline cold wallet.
- **Offline cold wallet.** The treasury's long-term wallet, which created the ONAGI token, has its key kept offline and on none of Onagi's servers.
- **Separate buyback key.** The buyback wallet uses its own key.

## What Onagi will never ask for

Onagi will never ask for a recovery phrase or a private key. Anyone who asks is a scammer, whatever they claim to be. The same goes for a user's Telegram link code.

## Reporting a vulnerability

See [SECURITY.md](../SECURITY.md).

---

[Back to README](../README.md) · Full reference: [onagi.si/docs](https://onagi.si/docs)
