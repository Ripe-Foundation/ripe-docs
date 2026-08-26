---
description: Compare the assets you can deposit on Earn
---

# What You Can Deposit on Earn

The previous guides cover the [GREEN LP token](05-provide-liquidity.md) and [RIPE](06-get-and-lock-ripe.md). Here's the whole list, because all four kinds of Earn asset work the same way. Exact pairs and venues depend on your network; the table uses the ones in the screenshots.

| Earn asset | What it is | Where you get it |
| --- | --- | --- |
| **sGREEN** | [Savings GREEN](../earning-and-rewards/01-sgreen.md): GREEN whose value rises with protocol revenue | Borrow with Receive Token set to Savings GREEN, or swap using **Get sGREEN** |
| **GREEN LP** (GREEN/USDG here) | Your share of the GREEN stablecoin pool | Swap into it via **Get GREEN**, or add both sides on Curve via **Get GREEN/USDG LP** ([Get GREEN and Provide Liquidity](05-provide-liquidity.md)) |
| **RIPE** | The protocol's governance token | **Get RIPE**, or **Bridge RIPE** from another network ([Get RIPE and Lock It](06-get-and-lock-ripe.md)) |
| **RIPE LP** (RIPE/WETH here) | Your share of the RIPE pool | **Get RIPE/WETH LP**, which routes to Uniswap V2 |

The pattern is the same for all four: acquire the token using its **Get** link, then deposit it into Ripe. The Get link always points at the correct venue, so you never have to identify a pool yourself. Don't go hunting for these pools on your own.

Under the hood they split into two kinds. RIPE and RIPE LP go into the governance vault, which is about locks and points. sGREEN and GREEN LP go into a [Stability Pool](../earning-and-rewards/02-stability-pools.md), which is about buying liquidated collateral below market.

**Reading the yield.** Each asset shows two numbers stacked together: a headline **APY**, and beneath it a second line reading **+ [something]% Locked RIPE Rewards**. They mean different things and it's easy to read them as one figure.

The headline is what the asset itself earns. The second line is additional rewards paid in RIPE, and they're labelled locked for the reason described in [Get RIPE and Lock It](06-get-and-lock-ripe.md#how-claiming-rewards-applies-a-lock): most of what you claim is staked rather than sent to your wallet. Treat the headline as the part you can access and the second line as value you'll be waiting on.

Both are estimates. Both run high while pools are small, and both come down as deposits grow.

---

_Not covered here: executing [liquidations](../core-protocol/04-liquidations.md) (the Liquidations page is for advanced users who run them; [Borrow GREEN](03-borrow-green.md) covers how to avoid being on the receiving end), [bonds](../governance-and-economics/03-bonds.md), and the [RIPE Reserve Engine](../governance-and-economics/04-reserve-engine.md) — those are covered in the protocol guides._
