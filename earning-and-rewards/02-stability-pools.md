---
description: Buy Liquidated Stock Tokens Below Market
---

# Stability Pools: Buy Liquidated Stock Tokens Below Market

Forget hunting for dips. Forget timing the market. Forget competing with bots.

When a stock-token position gets liquidated, the Stability Pool buys the tokens at the liquidation spread and you own your share. Deposit once, and every liquidation routed through the pool is a purchase made on your behalf: a [stock token](../core-protocol/08-stock-tokens.md) at 90 cents on the dollar, WETH at 90 cents on the dollar, while your deposit keeps earning on the side.

This is wholesale liquidations, democratized.

> **Live terms live onchain.** Which assets route through a pool, liquidation fees, claim rewards, and whether redemption is switched on vary by deployment and change over time. Every number on this page is an example. [Params](https://params.ripe.finance) is the source of truth.

## The Core Proposition

Ripe settles liquidations through Stability Pools first and auctions second. That puts depositors first in line:

* **Passive.** Deposit once; the pool buys in every liquidation that routes to it.
* **Proportional.** Your shares are your slice of everything the pool holds.
* **No bots, no gas wars.** Deposit, wait, claim.
* **Below market.** The pool pays oracle value minus the liquidation spread.

And you earn three ways at once:

1. **Deposit-asset yield.** [sGREEN](01-sgreen.md) keeps earning protocol revenue while it sits unspent in the pool; LP tokens keep accruing their pool's trading fees.
2. **Liquidation proceeds.** Collateral bought below oracle price.
3. **[RIPE rewards](03-ripe-rewards.md)** on your pool position, from the stakers allocation.

## How Stability Pools Work

### What You Deposit

Two kinds of asset go in:

* **sGREEN.** Keeps earning until it's spent. When the pool buys collateral with it, the sGREEN is redeemed for GREEN and burned.
* **GREEN-pair LP tokens.** When spent, they go to the treasury.

GREEN itself can't be deposited, but it shows up in the pool as claimable after redemptions (below), and the pool spends it first in the next liquidation.

Your deposit converts to shares. Shares represent your slice of everything the pool holds: the unspent deposit asset plus all the liquidated collateral it has bought, valued at oracle prices.

```
Share value  = Pool value / Total shares
Pool value   = Unspent deposit asset + Claimable collateral (at oracle price)
Your value   = Your shares × Share value
```

Tiny claims stay "dormant" (claimable directly, but not counted in pool value) until they cross a small dollar threshold.

### The Liquidation Flow

When a position crosses its liquidation threshold:

1. **The liquidation contract checks the route.** The asset must allow Stability Pool settlement, and the pool must be able to take it: a usable price, room for another claim asset, and no reserved conflict in its deposit asset.
2. **The pool pays.** Claimable GREEN goes first, then the deposit asset: sGREEN redeemed and burned, LP tokens sent to the treasury.
3. **The borrower's debt drops** by the value the pool actually supplied.
4. **Anything left goes to auction.**

**Example:** A stock token is worth $100 at oracle price and the liquidation spread is 10%. The pool takes the token and supplies $90 of value. The borrower's debt falls by $90; the pool now holds a $100 token it paid $90 for.

## The Economics

### What the Pool Pays

The pool pays oracle value minus the liquidation spread, and the spread is never more than the base liquidation fee:

* **5% spread**: the pool pays $95 for $100 of collateral
* **10% spread**: the pool pays $90
* **15% spread**: the pool pays $85

Two honest exceptions. A liquidation charges its fee once, so if the same position needs a second pass, the fee is zero and the pool pays par. And if a position is underwater (collateral worth less than the debt), there's no room for a fee, so the spread is zero there too.

### What That Means Over Time

**Volatile markets.** More liquidations, more purchases below oracle price. Your position buys the dip without you.

**Quiet markets.** Few liquidations, but sGREEN yield and RIPE rewards keep coming.

**The basket.** Over time the pool accumulates whatever gets liquidated: stock tokens, WETH, other supported collateral, plus GREEN from redemptions. You own your share of all of it, and you decide what to claim and when.

## Claiming, Withdrawing, Redeeming

### Claiming Collateral

Claim your share of any collateral the pool holds:

* **Pick the asset** and cap the claim by USD value, or take your full share.
* **Batch claims** go in one transaction; the batch has to claim something or it reverts.
* **Auto-deposit.** Claimed assets can go straight into a Ripe vault as collateral.
* **Your own loan must be healthy** (if you have one) for a claim to go through.
* **Claim rewards.** If governance has set a RIPE-per-dollar rate, a claim mints RIPE and locks it in the governance vault, fully staked.

**Delegation:** grant `canClaimFromStabPool` and someone else can claim on your behalf. Claimed assets always go to you.

### Withdrawing Your Deposit

You withdraw the deposit asset (sGREEN or LP) from what the pool hasn't spent. If most of the pool's value is sitting in liquidated collateral, claim the collateral instead of waiting for deposit-asset liquidity to return.

One rule to know: if any of the pool's claimable assets loses its price, deposits, withdrawals, and claims pause until it's priced again. Ripe fails closed rather than guessing.

### GREEN Redemption

If governance has turned redemption on for an asset the pool holds, any GREEN holder can swap GREEN at $1 for the pool's collateral at oracle price. That's a peg defense for GREEN, and the pool keeps its value: in an sGREEN pool the incoming GREEN is wrapped into sGREEN; in an LP pool it becomes claimable GREEN.

## Why Participate

* **Yield seekers:** three income sources, no management.
* **Risk-conscious:** first in line, no lockup, and every purchase is priced off the oracle.
* **GREEN supporters:** you're the liquidity that keeps liquidations orderly, and you earn RIPE for it.

## The Liquidation Game, Simplified

Every market crash. Every overleveraged position. Every liquidation.

They flow through Stability Pools first. While others panic-sell, you're buying at the liquidation spread. While others chase yield, you're stacking three streams at once.

Deposit. Wait. Claim.

***

_Stop watching liquidations happen. Start owning them._

_For technical implementation details, see the_ [_StabilityPool Technical Documentation_](https://ripe-finance.gitbook.io/ripe-developers/core/stabilitypool)_._
