---
description: Proactive Debt Reduction Without Liquidation Penalties
---

# Deleverage: Reduce Risk Before It's Too Late

Getting liquidated sucks. The fees, the forced selling, the stress.

But what if you could reduce your debt before things get ugly? That's what deleveraging does. Sell some collateral, pay down debt, stay in control. No liquidation penalties. No keeper fees. Just a rational way to manage risk.

Even better: if you're using Ripe's stability pools, the protocol can automatically burn your GREEN or sGREEN to reduce your debt. Your own assets, used to protect your own position.

## Quick Overview

**What is Deleverage?**

Deleveraging is the voluntary (or protocol-assisted) sale of collateral to reduce debt. Unlike liquidation, it happens before you reach the danger zone and carries no penalties.

**Key Differences:**

| Aspect | Deleverage | Liquidation |
|--------|------------|-------------|
| When it happens | Before liquidation threshold | After liquidation threshold |
| Who triggers it | You, delegated addresses, or third parties (when eligible) | Anyone (keepers) |
| Penalties | None | 5-15% + keeper fees |
| Your control | You choose which assets | Protocol chooses for you |
| Remaining collateral | Maximized | Minimized by fees |

## When Can You Be Deleveraged?

### Self-Deleveraging (Always Available)

You can always deleverage your own position:

* Choose which assets to sell
* Specify exactly how much debt to repay
* No threshold requirements
* No permissions needed

### Third-Party Deleveraging (Redemption Zone)

When your position enters the **redemption zone**, others can help deleverage you:

* Triggered when collateral value falls below redemption threshold
* Third parties can initiate deleverage to restore your position health
* Limited to the amount needed to return you to a safe LTV
* Still no liquidation penalties applied

## How Deleveraging Works

When deleveraging occurs, assets are processed in priority order:

**Phase 1: Stability Pool Assets**

Your assets in stability pools are used first:

* **GREEN deposits**: Burned directly to reduce your debt
* **sGREEN deposits**: Redeemed to GREEN, then burned
* No slippage, no fees, no market impact
* Most efficient form of debt repayment

**Phase 2: Stablecoins and Other Collateral**

After stability pool assets, remaining collateral is processed:

* USDC, USDT, and other stablecoins transferred to Endaoment at 1:1 value
* No liquidation discount — immediate dollar-for-dollar debt reduction
* Other vault assets processed as needed

### Choosing Specific Assets

You can specify exactly which assets to deleverage:

```
Example: Deleverage with Specific Assets

You have:
- 10,000 USDC ($10,000)
- 5 ETH ($15,000)
- 1,000 sGREEN ($1,000)

Debt: $12,000

You choose to deleverage with:
1. sGREEN first ($1,000 debt reduction)
2. USDC second ($10,000 debt reduction)
3. Keep all ETH

Result: $1,000 debt remaining, all ETH preserved
```

## Using [Underscore](https://underscore.finance/) Vaults?

If you're depositing into Underscore's AI-powered vaults, you don't need to worry about deleveraging. The vault manages its own Ripe position — borrowing, collateral, deleveraging — all handled automatically. When you withdraw, the vault adjusts its position behind the scenes.

For details on how Underscore vaults interact with Ripe, see [Underscore Protocol Integration](02-borrowing.md#underscore-protocol-integration).

## Delegation for Deleveraging

You can authorize others to manage your position:

### Available Permissions

* **canBorrow**: Allows delegate to deleverage on your behalf (uses same permission as borrowing)
* Trusted addresses can specify asset order and amounts
* Protocol contracts (like Underscore) can deleverage for operational needs

### Setting Up Delegation

Delegated addresses can:

* Initiate deleverage when your position is at risk
* Choose which assets to sell and in what order
* Maintain your position health automatically

## When to Deleverage

Don't wait for liquidation. Consider deleveraging when:

* Market volatility increases
* Your LTV approaches the warning zone
* You want to reduce exposure to specific assets
* You're rotating into different collateral

## How Deleverage Fits In

Deleverage is one of several protective mechanisms, each activating at different risk levels:

```
Healthy Zone → Warning Zone → Redemption Zone → Liquidation Zone
     |              |               |                  |
  No action    Can't borrow    Redemption +       Liquidation
   needed        more         Deleverage active    triggered
```

* **Redemption**: GREEN holders can redeem your collateral at $1 (no penalty to you)
* **Deleverage**: Your assets reduce your debt (no penalties, you choose which assets)
* **Liquidation**: Forced sale with 5-15% fees (last resort)

Deleverage is always available for yourself. Third parties can help deleverage you once you enter the redemption zone. For the complete picture, see [Liquidations](04-liquidations.md).

## Taking Control of Your Risk

Liquidation is a worst-case scenario. Deleverage is a tool.

Smart borrowers:

1. Monitor their position health
2. Set up delegation for automated protection
3. Use stability pools for instant debt reduction
4. Deleverage proactively rather than reactively

The protocol doesn't penalize you for managing your risk. It rewards it with zero-fee debt reduction using your own assets.

Stay ahead of the liquidation threshold. Stay in control.

***

_For technical implementation details, see the_ [_Deleverage Technical Documentation_](https://ripe-finance.gitbook.io/ripe-developers/core-lending/deleverage)_._
