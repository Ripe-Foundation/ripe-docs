---
description: Compare the assets you can deposit on Earn
---

# What You Can Deposit on Earn

The previous guides cover [GREEN/USDG LP](05-provide-liquidity.md) and [RIPE](06-get-and-lock-ripe.md). The examples below explain the different vault mechanics behind common Earn rows; the exact assets and venues exposed by the live interface can change.

| Example Earn asset | What it is | Example acquisition route |
| --- | --- | --- |
| **sGREEN** | [Savings Green USD](../earning-and-rewards/01-sgreen.md), a non-rebasing share whose GREEN backing can grow when configured protocol revenue reaches the vault | Borrow with Receive Token set to Savings GREEN, or swap using **Get sGREEN** |
| **GREEN/USDG LP** | Your share of the GREEN/USDG pool | Swap into it via **Get GREEN**, or add both sides on Curve via **Get GREEN/USDG LP** ([Get GREEN and Provide Liquidity](05-provide-liquidity.md)) |
| **RIPE** | The protocol's governance token | **Get RIPE**, or use **Bridge RIPE** where the interface offers a supported route ([Get RIPE and Lock It](06-get-and-lock-ripe.md)) |
| **RIPE/WETH LP** | Your share of the RIPE/WETH pool | **Get RIPE/WETH LP**, which routes to Uniswap V2 |

The interface pattern is the same: acquire the token using its **Get** link, then deposit it into Ripe. Under the hood, RIPE and RIPE/WETH LP use governance-vault lock and points accounting, while sGREEN and GREEN/USDG LP use Stability Pool accounting and can be exchanged for claimable collateral during liquidations. The Get link points at the intended venue, so you do not have to identify a pool yourself.

**Reading the yield.** Each asset shows two numbers stacked together: a headline **APY**, and beneath it a second line reading **+ [something]% Locked RIPE Rewards**. They mean different things and it's easy to read them as one figure.

The headline is what the asset itself earns. The second line is additional rewards paid in RIPE, with the claim behavior described in [Get RIPE and Lock It](06-get-and-lock-ripe.md#how-claiming-rewards-applies-a-lock): the configured auto-stake portion is deposited with a reward lock, while the remaining portion goes to your wallet. Read the live claim preview rather than assuming a fixed split.

Displayed rates are estimates and can move independently as protocol revenue, reward allowances and configuration, prices, claim assets, and participation change.

---

_Not covered here: executing [liquidations](../core-protocol/04-liquidations.md) (the Liquidations page is for advanced users who run them; [Borrow GREEN](03-borrow-green.md) covers how to avoid being on the receiving end), purchasing [bonds](../governance-and-economics/03-bonds.md), or creating a vesting position through the [RIPE Reserve Engine](../governance-and-economics/04-reserve-engine.md). Those distribution mechanisms are covered in the protocol guides rather than this Earn tutorial._
