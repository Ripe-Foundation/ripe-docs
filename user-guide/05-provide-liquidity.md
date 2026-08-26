---
description: Get GREEN and provide liquidity that may earn RIPE when configured
---

# Get GREEN and Provide Liquidity

This walkthrough uses the GREEN/USDG Curve pool shown in the captured interface as an example. Current pairs, venues, Stability eligibility, and reward configuration can differ by network and over time; check the interface and [RIPE Params](https://params.ripe.finance) for the current configuration. In this example, you provide liquidity to receive the LP token, then deposit that token on Earn to participate in Stability and earn [RIPE rewards](../earning-and-rewards/03-ripe-rewards.md) when configured.

**Step 1.** Get GREEN. Two ways: [borrow it](03-borrow-green.md), or press **Get GREEN** on the GREEN page and swap for it.

![The GREEN page](../.gitbook/assets/user-guide-04-green-page.png)

**Step 2.** In this example, get USDG by swapping for it on a DEX or bringing it across the bridge.

**Step 3.** Turn what you hold into the example LP token. The captured interface shows two routes:

* The quick route: press **Get GREEN** and set the swap's To asset to **GREEN/USDG LP**. One swap takes you from USDG straight to the LP token, both sides of the pool handled for you.
* The manual route: press **Get GREEN/USDG LP**. This takes you to the pool on Curve, outside the Ripe app. Add your GREEN and USDG there yourself; entering with only one side works but costs slippage, so bring both.

Either way you end up with LP tokens in your wallet representing your share of the pool.

![The GREEN/USDG pool on Curve](../.gitbook/assets/user-guide-12-curve-pool.png)

**Step 4.** This is the step people miss: come back to Ripe, go to the **Earn** page, find the configured LP row—GREEN/USDG LP in this example—and press **Deposit**. The deposited position can earn RIPE when configured and can be used as Stability liquidity in exchange for claimable liquidation collateral.

![Depositing your LP tokens into Ripe](../.gitbook/assets/user-guide-11-lp-deposit-modal.png)

Once deposited, the card shows your position, yield, and any configured RIPE rewards. Liquidation settlement can turn part of the vault's LP liquidity into claimable collateral, which is not the same as immediately withdrawable LP tokens.

![The GREEN/USDG Stability Pool card](../.gitbook/assets/user-guide-13-lp-stability-card.png)

A shortcut worth knowing: if you're borrowing anyway, the borrow dialog can send your GREEN straight to the Stability Pool, skipping the wallet round-trip.

If that deposit box shows "Your balance: -", it means you don't hold the token yet. Go back and do Step 3 first.

![A deposit dialog for a token you don't hold yet](../.gitbook/assets/user-guide-05-deposit-modal-empty.png)

![The Earn page](../.gitbook/assets/user-guide-03-earn-page.png)

**Step 5.** When this position is configured for RIPE rewards, its entitlement accrues and appears on your dashboard. Claiming runs through the [claim dialog described in the next guide](06-get-and-lock-ripe.md#how-claiming-rewards-applies-a-lock). The configured auto-stake portion receives a reward lock, and **Stake All** can deposit the full claim. Read that section before you claim for the first time.

A note on the yield numbers you'll see: young pools show very high rates because deposits are still small, and those rates come down as the pool grows.

Next: [Get RIPE and lock it](06-get-and-lock-ripe.md).
