---
description: Proactive Debt Reduction Without Liquidation Penalties
---

# Deleverage: Reduce Risk Before It's Too Late

Getting liquidated sucks. The fees, the forced selling, the stress.

But what if you could reduce your debt before things get ugly? That's what deleveraging does. Sell some collateral, pay down debt, stay in control. No liquidation penalties. No keeper fees. Just a rational way to manage risk.

Even better: configured deleverage routes can consume eligible assets already deposited in Ripe. An eligible sGREEN Stability position can be redeemed to GREEN and burned, while supported stable-side collateral can be transferred for debt credit. Paying GREEN from a wallet is standard repayment instead; GREEN is not an ordinary Stability deposit.

## Quick Overview

**What is Deleverage?**

Deleveraging is the voluntary (or protocol-assisted) use of eligible deposited assets to reduce debt. Unlike liquidation, it can happen before you reach the danger zone and carries no liquidation penalties.

If a debt-bearing collateral balance has no usable price or is nominally present in a vault with no usable backing, the account is quarantined and ordinary deleverage routes decline it. Standard GREEN repayment remains available while pricing or backing is restored.

**Key Differences:**

| Aspect | Deleverage | Liquidation |
|--------|------------|-------------|
| When it happens | Before liquidation threshold | After liquidation threshold |
| Who triggers it | You, delegated addresses, or third parties (when eligible) | Anyone (keepers) |
| Penalties | None | Configured liquidation + keeper fees |
| Your control | Specific-assets route lets an authorized caller choose | Protocol chooses for you |
| Remaining collateral | Maximized | Minimized by fees |

## When Can You Be Deleveraged?

### Self-Deleveraging with Specific Assets

Through the specific-assets route, an account owner can choose the asset order and target repayment without first entering the redemption zone, when the protocol can value and process the selected assets:

* Choose which assets to sell
* Specify the target amount of debt to repay
* No redemption-zone threshold requirement
* No permissions needed

### Third-Party Deleveraging (Redemption Zone)

When your position enters the **redemption zone**, others can help deleverage you:

* Triggered when collateral value is at or below the redemption threshold
* Third parties can initiate deleverage to restore your position health
* Limited to the amount needed to return you to a safe LTV
* Still no liquidation penalties applied

Broad deleverage evaluates authorization separately for each account; authority over one account does not carry over to another account in the same batch. The broad route has different authorization from the specific-assets route: an ordinary owner self-call is not automatically trusted, and an untrusted call proceeds only when the account is in the redemption zone and remains capped by the health-restoration calculation.

## How Deleveraging Works

When deleveraging occurs, assets are processed in priority order:

**Phase 1: Eligible Stability Positions**

Eligible positions in Stability vaults are considered first when their configured route is available:

* **sGREEN positions**: Eligible sGREEN can be redeemed to GREEN, then burned to reduce debt
* **Other configured Stability assets**: Used only when their asset configuration supplies an eligible deleverage route
* **Wallet GREEN**: Repaid through the standard repayment flow, not treated as a Stability deposit or collateral-based deleverage asset

Unavailable Stability liquidity is skipped instead of blocking the broader deleverage route.

**Phase 2: Stablecoins and Other Collateral**

After stability pool assets, remaining collateral is processed:

* Configured stable-side assets—for example, supported stablecoins—transferred to Endaoment at their credited value
* No liquidation discount — debt reduction follows the credited oracle value, subject to transfer and rounding behavior
* Other vault assets processed as needed

### Choosing Specific Assets

You can specify exactly which assets to deleverage:

The owner, an approved protocol caller, or a `canBorrow` delegate can choose the asset order. Underscore registration alone does not authorize another user's account.

```
Hypothetical example: preserve a Stock Token with specific assets

You have:
- Stock Token A ($15,000)
- Eligible stable-side collateral ($10,000)
- An eligible sGREEN Stability position ($1,000)

Debt: $12,000

You choose to deleverage with:
1. sGREEN first ($1,000 debt reduction)
2. The stable-side asset second ($10,000 debt reduction)
3. Preserve Stock Token A

Result: $1,000 debt remaining and Stock Token A preserved
```

That outcome depends on the selected assets and routes being eligible and the account holding enough of those assets. If you instead repay with GREEN from your wallet, standard repayment can reduce the debt without consuming deposited collateral; that is a separate path.

## Using [Underscore](https://underscore.finance/) Vaults?

If you're depositing into Underscore's AI-powered vaults, you don't need to worry about deleveraging. The vault manages its own Ripe position — borrowing, collateral, deleveraging — all handled automatically. When you withdraw, the vault adjusts its position behind the scenes.

For details on how Underscore vaults interact with Ripe, see [Underscore Protocol Integration](02-borrowing.md#underscore-earn-vault-integration).

## Delegation for Deleveraging

You can authorize others to manage your position:

### Available Permissions

* **canBorrow**: Allows delegate to deleverage on your behalf (uses same permission as borrowing)
* Trusted addresses can specify asset order and amounts; in a broad multi-user call, authority is evaluated separately for each user
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

* **Redemption**: GREEN is treated as a $1 debt-value input when eligible collateral is sized and credited (no liquidation penalty)
* **Deleverage**: Your assets reduce your debt without liquidation penalties; the specific-assets route lets an authorized caller choose the order
* **Liquidation**: Forced sale with configured liquidation and keeper fees (last resort)

An owner using the specific-assets route can self-deleverage before entering the redemption zone. The broad multi-user route evaluates authority separately for each account; an ordinary owner self-call follows the redemption-zone cap unless the caller is a trusted protocol or registered Underscore address. Immediately before settlement, the protocol re-reads the account's debt; if collateral interaction changed that debt, the entire deleverage transaction reverts. For the complete picture, see [Liquidations](04-liquidations.md).

## Taking Control of Your Risk

Liquidation is a worst-case scenario. Deleverage is a tool.

Smart borrowers:

1. Monitor their position health
2. Set up delegation for automated protection
3. Keep eligible sGREEN or stable-side collateral available for configured debt-reduction routes
4. Deleverage proactively rather than reactively

The protocol doesn't penalize you for managing your risk. It rewards it with zero-fee debt reduction using your own assets.

Stay ahead of the liquidation threshold. Stay in control.

***

_For technical implementation details, see the_ [_Deleverage Technical Documentation_](https://ripe-finance.gitbook.io/ripe-developers/core-lending/deleverage)_._
