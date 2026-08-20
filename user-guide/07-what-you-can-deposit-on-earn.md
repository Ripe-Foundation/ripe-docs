---
description: Compare the four assets you can deposit on Earn
---

# What You Can Deposit on Earn

The previous guides cover [GREEN/USDG LP](05-provide-liquidity.md) and [RIPE](06-get-and-lock-ripe.md). Here's the whole list, because all four assets work the same way.

| Earn asset | What it is | Where you get it |
| --- | --- | --- |
| **sGREEN** | [Savings Green USD](../earning-and-rewards/01-sgreen.md), which is GREEN that earns interest paid by borrowers | Borrow with Receive Token set to Savings GREEN, or swap using **Get sGREEN** |
| **GREEN/USDG LP** | Your share of the GREEN/USDG pool | Swap into it via **Get GREEN**, or add both sides on Curve via **Get GREEN/USDG LP** ([Get GREEN and Provide Liquidity](05-provide-liquidity.md)) |
| **RIPE** | The protocol's governance token | **Get RIPE**, or bridge it from Base ([Get RIPE and Lock It](06-get-and-lock-ripe.md)) |
| **RIPE/WETH LP** | Your share of the RIPE/WETH pool | **Get RIPE/WETH LP**, which routes to Uniswap V2 |

The pattern is the same for all four: acquire the token using its **Get** link, then deposit it into Ripe. The Get link always points at the correct venue, so you never have to identify a pool yourself. Don't go hunting for these pools on your own.

**Reading the yield.** Each asset shows two numbers stacked together: a headline **APY**, and beneath it a second line reading **+ [something]% Locked RIPE Rewards**. They mean different things and it's easy to read them as one figure.

The headline is what the asset itself earns. The second line is additional rewards paid in RIPE, and they're labelled locked for the reason described in [Get RIPE and Lock It](06-get-and-lock-ripe.md#claiming-rewards-always-locks-something): most of what you claim is staked rather than sent to your wallet. Treat the headline as the part you can access and the second line as value you'll be waiting on.

Both run very high while pools are small, and both fall as deposits grow.

---

_Not covered here: executing [liquidations](../core-protocol/04-liquidations.md) (the Liquidations page is for advanced users who run them; [Borrow GREEN](03-borrow-green.md) covers how to avoid being on the receiving end) and [bonds](../governance-and-economics/03-bonds.md) (paused right now; a guide comes when they return)._
