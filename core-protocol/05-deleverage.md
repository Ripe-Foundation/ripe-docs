---
description: Pay down debt with your own assets. No fee.
---

# Deleverage: Reduce Risk Before It's Too Late

Getting liquidated sucks. The fees, the forced selling, the stress.

Deleveraging is the alternative: use the sGREEN and stablecoin positions you already hold in Ripe to pay down debt. No liquidation fee. No keeper fee. Your own assets protecting your own position, and your stock tokens stay out of it.

> **Live terms live onchain.** Which assets can be used for deleverage, and the buffers and limits around it, vary by deployment and change over time. Every number on this page is an example. [Params](https://params.ripe.finance) is the source of truth.

## Quick Overview

Two ways: **specific assets** (you choose what to use and how much to repay) and **broad** (others deleverage you once you're in the redemption zone).

**Key Differences:**

| Aspect | Deleverage | Liquidation |
|--------|------------|-------------|
| When | Specific assets: any time, even during liquidation. Broad: once you're in the redemption zone | At or past the liquidation threshold |
| Who triggers it | Specific assets: you, a `canBorrow` delegate, or a trusted protocol. Broad: anyone | Anyone (keepers) |
| What's used | Your sGREEN and stablecoin positions | Stock tokens, WETH, other volatile collateral |
| Fees | None | Liquidation fee + keeper fee |
| Your control | You pick the assets and the amount | The protocol picks |

Paying GREEN from your wallet is ordinary repayment, not deleverage.

## When Can You Be Deleveraged?

### Self-Deleveraging with Specific Assets

You choose which of your sGREEN or stablecoin positions to use, in what order, and how much debt to repay. It works any time: no threshold, no permission needed, even while you're in liquidation. A delegate you've given `canBorrow`, or a trusted protocol, can do the same for you.

### Third-Party Deleveraging (Redemption Zone)

Once your collateral value is at or below your redemption threshold, anyone can deleverage you through the broad path:

* Capped at the debt reduction that restores your health; nobody can take more
* Still no fee
* Not available while you're in liquidation

If you call the broad path on your own account, you're treated like a third party: same zone requirement, same cap. Use specific assets instead.

## How Deleveraging Works

Assets are used in a fixed order:

**Phase 1: Your Stability Pool Positions**

* **sGREEN** is redeemed to GREEN and burned
* **LP tokens** are transferred to the treasury at oracle value
* Each dollar cancels a dollar of debt

**Phase 2: Stablecoin Collateral**

* USDC and other stablecoins are transferred to the treasury at oracle value
* No discount

Assets without a deleverage path are skipped. Stock tokens and other volatile collateral aren't used, so deleverage protects your stock position; it doesn't spend it. (Governance holds two emergency tools that can move volatile collateral at oracle value — a volatile-asset deleverage and a collateral swap — and only governance can run them.)

Just before settlement, Ripe re-reads your debt; if anything changed it mid-transaction, the whole thing reverts.

### Choosing Specific Assets

```
Example: keep the stock token

You hold:
- A stock token ($15,000)
- 10,000 USDC ($10,000)
- 1,000 sGREEN in a Stability Pool ($1,000)

Debt: $12,000

You deleverage with:
1. sGREEN first   → debt $11,000
2. USDC second    → debt $1,000
3. Stock token    → untouched

Result: $1,000 debt, stock token intact
```

### Full-Payoff Buffers and Dust

When you or a trusted caller pay off the whole debt, Ripe can take a sliver of collateral above the debt so the payoff lands exactly; the extra is capped both by a fixed amount and by a share of the debt, and in practice it is rounding dust. The contracts also carry a dust-forgiveness path for a payoff that lands a hair short, but its caps ship at zero, so nothing is forgiven unless governance turns it on. Neither applies when the position belongs to an Underscore Earn vault.

## Using [Underscore](https://underscore.finance/) Vaults?

An Underscore vault manages its own Ripe position, deleveraging included; how it borrows, deleverages, and adjusts when you withdraw depends on that vault. See [Underscore Protocol Integration](02-borrowing.md#underscore-earn-vault-integration).

## Delegation for Deleveraging

* **`canBorrow`** lets a delegate deleverage for you with specific assets; it's the same flag that lets them borrow
* Delegates pick the assets, the order, and the amount
* In a batch that covers several accounts, each account's permission is checked on its own

## When to Deleverage

Don't wait for liquidation. Deleverage when:

* Volatility picks up
* Your LTV nears the warning zone
* You want less exposure to one asset
* You're rotating collateral

## How Deleverage Fits In

```
Healthy → Warning → Redemption Zone → Liquidation Zone
   |         |            |                  |
Deleverage  Deleverage   + redemption      Liquidation, and you
any time    any time     + broad deleverage  can still deleverage
```

* **Redemption**: GREEN holders pay your debt at $1 and take collateral at oracle price
* **Deleverage**: your sGREEN and stablecoins pay your debt, no fee, your choice of assets
* **Liquidation**: forced sale with fees, the last resort

For the full picture, see [Liquidations](04-liquidations.md).

## Taking Control of Your Risk

Liquidation is a worst case. Deleverage is a tool.

1. Watch your position
2. Keep some sGREEN in a Stability Pool or stablecoins in your position; that's your deleverage fuel
3. Set up delegation if you want someone else able to act
4. Deleverage early, not late

Stay ahead of the liquidation threshold. Stay in control.

***

_For technical implementation details, see the_ [_Deleverage Technical Documentation_](https://ripe-finance.gitbook.io/ripe-developers/core/deleverage)_._
