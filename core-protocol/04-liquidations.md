---
description: A slice, not the plate.
---

# Liquidations: A Slice, Not the Plate

Most protocols liquidate everything the moment you cross the line. Position worth $10,000, debt at $9,001? They take it all.

Ripe takes only what it needs to make the position healthy, and you keep the rest. The honest limit: if the shortfall is severe, the target becomes your full debt, and all liquidation-eligible collateral can go and still leave debt behind. This page is about staying on the right side of that line, and what happens if you don't.

> **Live terms live onchain.** Assets, LTVs, thresholds, fees, auction settings, and which optional features are switched on vary by deployment and change over time. Every number on this page is an example. [Params](https://params.ripe.finance) is the source of truth.

## Executive Summary

**Key Points:**

* 🛡️ **Two warning zones first**: [Deleverage](05-deleverage.md) and redemption give you room before anything is forced
* 📊 **A slice, not the plate**: Liquidation targets the debt reduction that restores health, not your whole position
* 💰 **Bounded fees**: A base fee plus a keeper fee, charged once per liquidation and capped by your collateral surplus
* ⚡ **Anyone can trigger it**: An open keeper network, paid in sGREEN
* 🚫 **Your stablecoins and sGREEN aren't sold in a liquidation**: They're set up for [deleverage](05-deleverage.md) instead

**Quick Visual: The Liquidation Flow**

```
Your position slips
        ↓
DELEVERAGE (you, any time)
└─ Your sGREEN and stablecoin positions pay down debt. No fee.
        ↓
REDEMPTION ZONE
└─ GREEN holders pay your debt at $1 and take collateral
   at oracle price. No discount, no fee.
        ↓
LIQUIDATION THRESHOLD — anyone can trigger
        ↓
┌───────────────────────────────────────┐
│ PHASE 1: Stability Pool Swaps         │
│ • Stock tokens, WETH, other volatiles │
│ • Pool buys at the liquidation spread │
└──────────────────┬────────────────────┘
                   ↓ (whatever's left)
┌───────────────────────────────────────┐
│ PHASE 2: Dutch Auction                │
│ • Anyone buys with GREEN              │
│ • Discount grows over the window      │
└───────────────────────────────────────┘
```

**What This Means For You:**

* ✅ Deleverage early and liquidation never starts
* ✅ Keep the collateral that isn't needed
* ✅ No market dumps: pools first, auctions second
* ✅ Your stablecoins and sGREEN stay put

## Quick Navigation

**Understanding the Basics:**

* [Why Liquidations Matter](04-liquidations.md#why-liquidations-matter) - Protocol safety and borrower protection
* [Risk Zones](04-liquidations.md#understanding-the-risk-zones) - Three thresholds and visual guide
* [Deleverage First](05-deleverage.md) - Proactive debt reduction (separate page)
* [Redemption Buffer](04-liquidations.md#the-redemption-buffer) - Your first line of defense

**The Liquidation Process:**

* [Phase 1: Stability Pools](04-liquidations.md#phase-1-stability-pool-swaps) - Pools buy at the liquidation spread
* [Phase 2: Dutch Auctions](04-liquidations.md#phase-2-dutch-auctions) - Time-based discounts

**Advanced Topics:**

* [Liquidation Economics](04-liquidations.md#liquidation-economics) - Fees and calculations
* [Keeper Network](04-liquidations.md#the-keeper-network) - Open execution
* [Bad Debt Handling](04-liquidations.md#what-if-bad-debt-occurs) - Last resort measures

***

## Why Liquidations Matter

### Protecting Protocol Solvency

Every GREEN is minted against collateral. When a position's collateral falls, or interest piles up, until the debt is no longer covered, something has to close the gap or GREEN loses its backing. Liquidation is that something. It exists to protect [GREEN](01-green-stablecoin.md) and everyone who holds it.

### The Borrower-Friendly Approach

The gap is closed with a target, not a wipeout. Ripe works out how much debt has to be repaid to bring the position back to a safe LTV and takes only the collateral that covers it.

* **You keep the rest** — collateral not needed to hit the target stays yours
* **Less selling** — smaller liquidations mean less pressure on the assets
* **Room to recover** — the position survives, so a bounce is still yours
* **Fixed, capped fees** — no arbitrary penalties

## Understanding the Risk Zones

### The Three Critical Thresholds

Each asset carries three numbers, and your position's [weighted terms](02-borrowing.md#weighted-debt-terms-explained) blend them into one of each:

**1. Maximum LTV (Loan-to-Value)**

* Your borrowing limit as a share of collateral value
* Example: 70% LTV on $10,000 of collateral = $7,000 of GREEN
* Past it, you can't borrow more

**2. Redemption Threshold**

* The early warning: GREEN holders can now pay down your debt and take collateral at oracle price
* Example: 77%

**3. Liquidation Threshold**

* Past this, anyone can liquidate you
* Example: 80% — with $8,000 of debt, liquidation opens once collateral is worth $10,000 or less

### How Risk Escalates: Visual Zone Map

Example: $6,000 of debt against collateral that starts at $10,000, using 70% / 77% / 80% thresholds:

```
POSITION HEALTH VISUALIZATION (for $6,000 debt)
←─────────────────────────────────────────────────────────────→
$10,000                    $8,571      $7,792     $7,500      $0
  YOU                        ↓           ↓           ↓
  ARE                   Max LTV     Redemption  Liquidation
  HERE                   (70%)        (77%)        (80%)

[════ SAFE ZONE ════][CAUTION][REDEMPTION][LIQUIDATION]
     ✅ Healthy        ⚠️ Warning  🚨 Danger    💀 Critical
```

**🟢 Zone 1: Healthy** (collateral above $8,571)

* Under your max LTV; you can still borrow
* Nobody can touch the position

**🟡 Zone 2: Warning** ($8,571 – $7,792)

* Over your max LTV: no new borrows, and no withdrawals until you're back under it
* Nobody can touch your collateral yet
* Add collateral or repay

**🟠 Zone 3: Redemption** ($7,792 – $7,500)

* GREEN holders can redeem against your collateral, and third parties can [deleverage](05-deleverage.md) you
* Both reduce your debt at fair value, no fee

**🔴 Zone 4: Liquidation** ($7,500 and below)

* Anyone can trigger a liquidation
* Fees apply

Thresholds vary by asset: stablecoin-tier assets sit higher, volatile assets lower. _For how the three thresholds combine, see_ [_Understanding Three Thresholds_](02-borrowing.md#how-thresholds-work-together-a-visual-guide)_._

## The Redemption Buffer

### Your First Line of Defense

Once you're in the redemption zone, and if governance has turned redemption on for an asset, any GREEN holder can redeem against your position:

* They pay GREEN at $1 and receive your collateral at oracle price — no discount
* Your debt drops by the same amount
* It's capped at what brings you back to target, so nobody can redeem more than your position needs
* It may pull you out of the zone before a liquidation ever becomes possible

A few rules: the redeemer can't be you; assets with recipient checks only go to addresses that pass them; and redemption is off while you're in liquidation. Registered Underscore Earn vaults are skipped by both redemption and liquidation.

Redemptions also help hold GREEN at $1 during stress. **Want to move first?** [Deleverage](05-deleverage.md#self-deleveraging-with-specific-assets) with your sGREEN or stablecoin positions at any time.

## The Liquidation Process

### Before Liquidation: Deleverage

Two ways to bring debt down with no fee at all:

* **Repay GREEN** from your wallet, the ordinary way
* **[Deleverage](05-deleverage.md)**: your sGREEN Stability Pool positions are redeemed and burned, and your stablecoin collateral goes to the treasury at oracle value, each dollar cancelling a dollar of debt

You, a `canBorrow` delegate, or a trusted protocol can deleverage with specific assets at any time, even during a liquidation. Third parties can deleverage you once you're in the redemption zone, capped at what restores your health.

**Key point:** liquidation only handles assets that have a Stability Pool or auction path. Stablecoins and sGREEN are set up for deleverage instead, so a liquidation leaves them alone.

### What Happens When Liquidation Starts

Once you're at or past the liquidation threshold, anyone can trigger a liquidation (governance also holds a kill switch that pauses all liquidations). From that point:

1. **Your account is frozen.** No new borrows, no withdrawals (zero-LTV assets included). You can still repay, and you can still deleverage with your own sGREEN and stablecoin positions.
2. **Fees are set once.** The base liquidation fee and the keeper fee are calculated on the first pass of a liquidation episode. Retry passes charge nothing — see [Liquidation Economics](#liquidation-economics).
3. **Collateral is processed toward the target.** Stability Pools first, then auctions for what's left. Collateral that isn't needed stays with you.
4. **An open auction owns the pass.** While one of your auctions is running, another liquidation pass has to wait. Once it expires, anyone can clear it so liquidation can retry (a governance-paused auction stays until governance clears it).

**Getting out.** Any update that finds your position healthy clears the liquidation flag and cancels your open auctions. Healthy means debt at or below your max borrow capacity, so repaying, adding collateral, or a price recovery plus any action that re-checks your position all count. Creeping back above the liquidation threshold alone isn't enough.

### Phase 1: Stability Pool Swaps

[Stability Pools](../earning-and-rewards/02-stability-pools.md) hold sGREEN and LP tokens and stand ready to buy liquidated collateral:

1. Your stock tokens, WETH, or other volatile collateral needs to be sold
2. The pool buys it at the liquidation spread; the sGREEN it pays with is burned, and any LP tokens go to the treasury
3. The pool's depositors own the collateral through their shares
4. Your debt drops by what the pool paid

An asset only goes to a pool if governance has enabled that path for it, and the pool must accept the asset and have capacity; otherwise it goes straight to auction. Assets with a whitelist settle through a dedicated permissioned pool. An asset can be auction-only, or pools-then-auction; the auction path always backs a pool path.

On a retry pass the base fee is zero, so the pool settles at par: no spread. [Stability Pools](../earning-and-rewards/02-stability-pools.md) covers the depositor side.

### Phase 2: Dutch Auctions

Whatever the pools didn't take goes to auction:

* The discount starts small and grows to the maximum over the auction window, after an optional start delay
* Anyone with GREEN can buy any amount at the current discount, settled on the spot
* Their GREEN is burned and your debt drops by that much
* Each purchase is capped by your remaining debt and remaining collateral
* The auction ends when the collateral is gone or your position is healthy again; unsold collateral stays in your vault

Example: a 1% starting discount rising to 50% by the end of the window.

### Stock Tokens

Stock tokens are liquidated like any other volatile collateral: through a pool first, if that path is on, then auction for the rest. A limited shortfall takes part of them; a severe one can take all of them. See [Which Events Can Move Your Tokens](08-stock-tokens.md#which-events-can-move-your-tokens).

## Liquidation Economics

### Understanding Liquidation Fees

Two fees, charged once per liquidation episode:

* **Base liquidation fee** — set per asset and blended across your position by borrowing power. Example tiers:
  * Stablecoin-tier: 5% (not sold in a liquidation; the rate only feeds your blended fee)
  * Blue-chip (WETH, WBTC): 10%
  * Higher-volatility assets: 15%
* **Keeper fee** — a small cut of debt, with a floor and a ceiling. Example: 1%

**The caps.** Together the fees can't exceed your collateral surplus (collateral value minus debt) or 99% of your debt. If they would, the keeper fee is cut first, then the base fee.

**How they're paid.** The Stability Pool's spread covers the base fee, and at most the base fee. Whatever the spread didn't cover, plus the keeper fee, is added to your debt. A retry pass charges no fees, and neither does a first pass that repays nothing and starts no auction.

**Where the value goes**

1. **Stability Pool depositors** — collateral below oracle price
2. **Keepers** — the keeper fee, minted as sGREEN (GREEN on request)
3. **Auction buyers** — collateral at the current discount
4. **The treasury** — LP tokens a pool spent; GREEN spent anywhere is burned

### Partial Liquidation Design

The target is the debt reduction that gets you back to a safe LTV. It's built from your live debt plus fees, your collateral value, the lowest LTV among your assets, and a payback buffer. Ripe takes the collateral that covers the target and nothing more.

Example:

```
Debt: $1,000
Collateral: $1,250 — past the threshold
Target LTV: 50%
Liquidation fee: 10%, no keeper fee

Target repayment = (debt + fee − collateral × 50%) ÷ (1 − 50%)
                 = ($1,000 + $100 − $625) ÷ 0.5 = $950

The pool pays $950 and takes $1,055.56 of collateral (10% below oracle price).
Debt falls to $50. Collateral left: $194.44 — back well inside the safe zone.
The $105.56 spread is the pool depositors' return.

Other protocols: take all $1,250. You get $0.
```

If the shortfall is severe, the target becomes the full debt and every asset with a liquidation path can be taken, with debt still remaining.

## The Keeper Network

### Your Automated Safety Net

Keepers are independent operators who watch positions and trigger liquidations when one crosses the line. Anyone can be one; no permission needed.

* They monitor positions around the clock and compete to act first
* They earn the keeper fee, minted as sGREEN
* Fast action means positions are handled while there's still surplus, which is what keeps your loss small

## Traditional DeFi vs Ripe

| Feature | Traditional DeFi | Ripe |
| --- | --- | --- |
| **Liquidation amount** | Entire position | Only what restores health; the full debt only in a severe shortfall |
| **Warning system** | None | Redemption and deleverage zones |
| **Liquidation fee** | 13–50% penalty | Base fee plus keeper fee, capped by your surplus |
| **Who can buy** | MEV bots | Anyone: Stability Pools, then auctions |
| **Proactive option** | None | Deleverage, no fee, any time |
| **Stablecoins and sGREEN** | Sold like everything else | Handled by deleverage, not liquidation |
| **Market impact** | Dumps | Pools first, then auctions |

## What If Bad Debt Occurs?

Debt left on a liquidated position isn't automatically protocol bad debt; governance records bad debt separately. Once recorded, [bond](../governance-and-economics/03-bonds.md) purchases can clear it, and the RIPE paid out for that counts toward the protocol-wide 1 billion cap rather than on top of it.

## Liquidations That Don't Ruin Lives

Elsewhere, a liquidation means starting over from zero. On Ripe you lose a slice, the position survives, and you're still in the game. With deleverage, you can usually skip the slice altogether.

That $194.44 in the example is the difference between a setback and a catastrophe. The protocol isn't being nice: borrowers who survive keep borrowing and keep paying interest. Your success is the protocol's success.

***

_For technical implementation details, see the_ [_AuctionHouse Technical Documentation_](https://ripe-finance.gitbook.io/ripe-developers/core/auctionhouse)_._
