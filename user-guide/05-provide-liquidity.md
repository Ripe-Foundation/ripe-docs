---
description: Get GREEN and earn RIPE by providing liquidity
---

# Get GREEN and Provide Liquidity

Liquidity providers in the GREEN stablecoin pool deposit their LP tokens into a [Stability Pool](../earning-and-rewards/02-stability-pools.md) and earn [RIPE rewards](../earning-and-rewards/03-ripe-rewards.md). The pair depends on your network; the screenshots show GREEN/USDG on Curve, so that's the example below.

**Step 1.** Get GREEN. Two ways: [borrow it](03-borrow-green.md), or press **Get GREEN** on the GREEN page and swap for it.

![The GREEN page](../.gitbook/assets/user-guide-04-green-page.png)

**Step 2.** Get the other side of the pair — USDG in this example. Swap for it on a DEX or bring it across the bridge.

**Step 3.** Turn what you hold into LP tokens. Two routes:

* The quick route: press **Get GREEN** and set the swap's To asset to the LP token (**GREEN/USDG LP** here). One swap takes you from the stablecoin straight to the LP token, both sides of the pool handled for you.
* The manual route: press **Get GREEN/USDG LP**. This takes you to the pool on Curve, outside the Ripe app. Add both sides there yourself; entering with only one side works but costs slippage, so bring both.

Either way you end up with LP tokens in your wallet representing your share of the pool.

![The GREEN/USDG pool on Curve](../.gitbook/assets/user-guide-12-curve-pool.png)

**Step 4.** This is the step people miss: come back to Ripe, go to the **Earn** page, find the LP row (marked Stability Pool) and press **Deposit**. LP tokens sitting in your wallet earn nothing from Ripe; deposited ones earn RIPE and take part in liquidations.

![Depositing your LP tokens into Ripe](../.gitbook/assets/user-guide-11-lp-deposit-modal.png)

Once deposited, the card shows your position, the yield, and the RIPE rewards on top. When a liquidation routes through the pool, some of the pool's LP liquidity is swapped for the liquidated collateral — you'll see it as claimable collateral rather than as LP tokens.

![The GREEN/USDG Stability Pool card](../.gitbook/assets/user-guide-13-lp-stability-card.png)

A shortcut worth knowing: if you're borrowing anyway, the borrow dialog can send Savings GREEN straight into the Stability Pool, skipping the wallet round-trip.

If that deposit box shows "Your balance: -", it means you don't hold the token yet. Go back and do Step 3 first.

![A deposit dialog for a token you don't hold yet](../.gitbook/assets/user-guide-05-deposit-modal-empty.png)

![The Earn page](../.gitbook/assets/user-guide-03-earn-page.png)

**Step 5.** Rewards accrue automatically and show on your dashboard. They're paid in RIPE, which means claiming them runs through the [claim dialog described in the next guide](06-get-and-lock-ripe.md#how-claiming-rewards-applies-a-lock), and that dialog stakes and locks part of what you claim. Read that section before you claim for the first time.

A note on the yield numbers you'll see: they're estimates. Young pools show very high rates because deposits are still small, and rates come down as the pool grows.

Next: [Get RIPE and lock it](06-get-and-lock-ripe.md).
