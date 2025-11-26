---
description: When Leverage Goes Wrong (But Not Too Wrong)
---

# Liquidations: When Leverage Goes Wrong (But Not Too Wrong)

Most protocols liquidate everything when you cross the line. Position worth $10,000? Debt at $9,001? They take it all. You get nothing.

Ripe takes only what's needed. Your position hits the danger zone? The protocol takes just enough collateral to restore health. You keep the rest.

Multiple layers of protection. Partial liquidations only. Proactive deleveraging available. This isn't charity — it's math. Keeping borrowers alive keeps the protocol healthy.

## Executive Summary

**Key Points:**

* 🛡️ **Multiple defense layers**: Deleverage → Redemption → Stability pools → Dutch auctions
* 📊 **Partial only**: Liquidates just enough to restore health, not entire position
* 💰 **Fair pricing**: 5-15% liquidation fees on volatile assets
* ⚡ **Automated**: Keeper network ensures rapid execution
* 🎯 **Proactive options**: [Deleverage](05-deleverage.md) your position before liquidation with zero penalties

**Quick Visual: The Liquidation Flow**

```
Your Position Becomes Unhealthy
            ↓
    DELEVERAGE (Proactive)
    └─ Reduce debt using your GREEN,
       sGREEN, or stablecoins (no penalty)
       See: Deleverage documentation
            ↓
    REDEMPTION ZONE
    └─ GREEN holders can redeem
       at $1 (no penalty for you)
            ↓
    LIQUIDATION TRIGGERED
            ↓
┌─────────────────────────────────┐
│   PHASE 1: Stability Pool Swaps │
│   • Volatile assets (ETH, BTC)  │
│   • Instant swaps at discount   │
│   • 5-15% liquidation fee       │
└────────────┬────────────────────┘
             ↓ (if needed)
┌───────────────────────────┐
│   PHASE 2: Dutch Auction  │
│   • Public sale           │
│   • Increasing discounts  │
│   • 1% → 50% over time    │
└───────────────────────────┘
```

**What This Means For You:**

* ✅ Deleverage proactively to avoid liquidation entirely
* ✅ Keep most of your collateral
* ✅ Lower fees than other protocols
* ✅ No cascade liquidations
* ✅ Fair, transparent process

## Quick Navigation

**Understanding the Basics:**

* [Why Liquidations Matter](04-liquidations.md#why-liquidations-matter) - Protocol safety and borrower protection
* [Risk Zones](04-liquidations.md#understanding-the-risk-zones) - Three thresholds and visual guide
* [Deleverage First](05-deleverage.md) - Proactive debt reduction (separate page)
* [Redemption Buffer](04-liquidations.md#the-redemption-buffer) - Your first line of defense

**The Liquidation Process:**

* [Phase 1: Stability Pools](04-liquidations.md#phase-1-stability-pool-swaps) - Instant liquidity for volatile assets
* [Phase 2: Dutch Auctions](04-liquidations.md#phase-2-dutch-auctions) - Time-based discounts

**Advanced Topics:**

* [Liquidation Economics](04-liquidations.md#liquidation-economics) - Fees and calculations
* [Keeper Network](04-liquidations.md#the-keeper-network) - Automated execution
* [Bad Debt Handling](04-liquidations.md#what-if-bad-debt-occurs) - Last resort measures

***

## Why Liquidations Matter

### Protecting Protocol Solvency

Liquidations serve as the critical mechanism ensuring that [GREEN](01-green-stablecoin.md) remains fully backed. When [borrowing positions](02-borrowing.md) become undercollateralized due to collateral value drops or accumulated interest, the protocol must act to prevent bad debt accumulation. Without effective liquidations, GREEN could lose its peg, affecting GREEN and sGREEN holders.

### The Borrower-Friendly Approach

Unlike traditional DeFi protocols that liquidate entire positions at once, Ripe only liquidates the minimum necessary to restore healthy collateralization. This approach:

* **Preserves User Value**: Keep as much collateral as possible
* **Reduces Market Impact**: Smaller liquidations mean less selling pressure
* **Enables Recovery**: Partial liquidations allow positions to potentially recover
* **Maintains Fairness**: Fixed fees rather than arbitrary penalties

## Understanding the Risk Zones

### The Three Critical Thresholds

Your position's safety depends on three key thresholds that work together to create graduated risk zones:

**1. Maximum LTV (Loan-to-Value)**

* Your borrowing limit as a percentage of collateral
* Example: 70% LTV on $10,000 = $7,000 maximum borrow
* Cannot borrow more once reached

**2. Redemption Threshold**

* Early warning system before liquidation
* GREEN holders can redeem against your position
* Provides market-based deleveraging opportunity
* Example: At 80% threshold, others can swap GREEN for your collateral at par

**3. Liquidation Threshold**

* The danger zone where forced liquidation begins
* Calculated as minimum collateral needed for your debt
* Example: At 90% threshold with $8,000 debt, liquidation starts when collateral < $8,889

### How Risk Escalates: Visual Zone Map

Consider a position with $10,000 initial collateral and $6,000 debt:

```
POSITION HEALTH VISUALIZATION (for $6,000 debt)
←─────────────────────────────────────────────────────────────→
$10,000                    $8,571      $7,500     $6,667      $0
  YOU                        ↓           ↓           ↓
  ARE                   Max LTV     Redemption  Liquidation
  HERE                   (70%)        (80%)        (90%)

[════ SAFE ZONE ════][CAUTION][REDEMPTION][LIQUIDATION]
     ✅ Healthy        ⚠️ Warning  🚨 Danger    💀 Critical
```

**Zone Breakdown:**

**🟢 Zone 1: Healthy** (Collateral > $8,571)

* Below 70% LTV maximum
* Can still borrow more
* No intervention possible

**🟡 Zone 2: Warning** (Collateral $8,571 - $7,500)

* Exceeded max LTV, cannot borrow
* Still safe from redemption
* Time to add collateral or repay

**🟠 Zone 3: Redemption** (Collateral $7,500 - $6,667)

* Redemption threshold breached
* GREEN holders can redeem your collateral
* Acts as automatic deleveraging

**🔴 Zone 4: Liquidation** (Collateral < $6,667)

* Liquidation threshold breached
* Multi-phase liquidation activates
* Position being actively liquidated

_For a detailed explanation of how these thresholds work together, see_ [_Understanding Three Thresholds_](02-borrowing.md#how-thresholds-work-together-a-visual-guide) _in the borrowing documentation._

## The Redemption Buffer

### Your First Line of Defense

Before liquidation becomes possible, the redemption mechanism provides a unique protective buffer. When your position enters the redemption zone:

* **GREEN holders can exchange** their tokens for your collateral at exactly $1 value
* **No discount applied** - fair value exchange protects you from penalties
* **Reduces your debt** automatically as collateral is redeemed
* **May prevent liquidation** by improving your position health

This mechanism serves dual purposes: protecting borrowers through gradual deleveraging while helping maintain GREEN's $1 peg during market stress.

**Want to take action before redemption?** Consider [deleveraging](05-deleverage.md) your position proactively. You can voluntarily reduce your debt by selling collateral — without liquidation penalties — at any time.

## The Liquidation Process

When liquidation becomes necessary, Ripe employs a carefully orchestrated two-phase approach designed to minimize impact while ensuring debt repayment.

### Before Liquidation: Deleverage

Before your position reaches liquidation, you have options to reduce debt with **zero penalties**:

* **[Deleverage](05-deleverage.md)** your position using GREEN, sGREEN, or stablecoins
* Burns your stability pool deposits to reduce debt directly
* Transfers stablecoins to Endaoment at 1:1 value
* No liquidation fees, no discounts — just debt reduction

This is handled by the separate [Deleverage](05-deleverage.md) system and can be triggered by you, delegated addresses, or third parties when you're in the redemption zone.

**Key Point**: GREEN, sGREEN, and stablecoins are NOT processed during liquidation itself — they're handled through deleveraging. Liquidation only deals with volatile assets.

### What Happens When Liquidation Starts

When your position crosses the liquidation threshold:

1. **Position enters `inLiquidation` state**
   * You are blocked from taking new borrows
   * You can still repay debt to exit liquidation
   * Your volatile collateral becomes eligible for processing

2. **Liquidation fees are calculated**
   * Base liquidation fee (5-15% depending on asset)
   * Keeper reward (1% of debt)
   * **Important**: Total fees are capped by your collateral surplus to ensure liquidation remains possible

3. **Two-phase asset processing begins**
   * Only enough collateral is taken to restore health
   * You keep any remaining collateral

4. **Exiting liquidation mode**
   * Repay your debt to exit `inLiquidation` state
   * Once debt health is restored, normal operations resume

### Phase 1: Stability Pool Swaps

The protocol engages [stability pools](../earning-and-rewards/06-stability-pools.md) for instant liquidity on your volatile assets (ETH, WBTC, etc.):

**How Pool Swaps Work**

1. Your collateral (ETH, WBTC, etc.) needs liquidation
2. [Stability pools](../earning-and-rewards/06-stability-pools.md) hold GREEN LP tokens and [sGREEN](../earning-and-rewards/05-sgreen.md)
3. Pool assets swap for your collateral at the liquidation discount
4. Pool participants get discounted assets, you avoid market dumps

**The Win-Win Dynamic**

* You avoid harsh market conditions and slippage
* Pool depositors earn fixed discounts (typically 5-15%)
* Protocol maintains orderly liquidations
* No dependence on external market depth

**Additional Pool Benefits**

* Depositors earn RIPE rewards from the Stakers allocation
* GREEN holders can redeem against pool collateral for peg stability
* Flexible withdrawal lets depositors choose which assets to claim

**Special Note on Permissioned Assets** For regulated assets (tokenized securities, real estate):

* Dedicated permissioned pools with whitelisted participants
* Same swap mechanics but restricted access
* Ensures compliance throughout liquidation process

_For deeper understanding of stability pool mechanics, see_ [_Stability Pools_](../earning-and-rewards/06-stability-pools.md)_._

### Phase 2: Dutch Auctions

For any remaining collateral after pools are exhausted:

**Time-Based Discounts**

* Auctions start with small discounts (1%)
* Discounts increase linearly over time (up to 50%)
* Anyone with GREEN can buy instantly at current discount
* No waiting for auction to "end" - immediate settlement

**Auction Mechanics**

* Initial delay prevents front-running
* Buyers can purchase any amount of fungible assets
* GREEN payment is burned, reducing your debt
* Auction ends when all collateral sold or debt repaid
* Any excess collateral value returns to you

## Liquidation Economics

### Understanding Liquidation Fees

Liquidation fees serve multiple purposes in the ecosystem:

**Fee Structure**

* Base liquidation fee: Typically 5-15% depending on asset risk
* Keeper rewards: Additional fee (1%) for liquidation executors
* Total impact: Your cost becomes others' profit opportunity

**Where Fees Go**

1. **Stability Pool Depositors**: Receive discounted collateral as compensation
2. **Keepers**: Earn rewards for monitoring and executing liquidations
3. **Auction Buyers**: Purchase collateral below market value
4. **Protocol**: No direct protocol extraction - all value flows to participants

### Partial Liquidation Design

Ripe's system only liquidates enough to restore healthy LTV:

**Target Calculation**

* Determines minimum repayment for safe LTV
* Adds small buffer (1-2%) for safety
* Preserves maximum collateral possible
* Single formula works across all liquidation types

> **💡 TL;DR**: If you're at 80% LTV and need to get back to 50%, we only liquidate exactly what's needed - not your entire position. In the example below, you keep 25% of your collateral instead of losing everything.

**Real Example: Partial vs Full Liquidation**

```
Your position hits liquidation threshold:
- Debt: $1,000
- Collateral: $1,250 (too risky!)
- Target: Get back to safe 50% LTV

Other protocols: Take ALL $1,250 → You get $0

Ripe Protocol:
- Takes only $937.50 of collateral
- Pays down debt to $156.25
- You keep $312.50 (25% of original!)
- Position restored to exactly 50% LTV

The 10% liquidation fee ($93.75) goes to stability pool depositors
as their profit for providing instant liquidity.
```

> **🎯 Key Takeaway**: You lost $937.50 but kept $312.50. On other protocols, you'd lose everything. That's a $312.50 difference in your pocket.

## The Keeper Network

### Your Automated Safety Net

Keepers are the protocol's decentralized guardians — independent operators who monitor positions and execute liquidations when needed. Think of them as automated lifeguards watching over the protocol.

**How Keepers Work**

* Monitor all positions 24/7 for liquidation thresholds
* Trigger liquidations the moment positions become unsafe
* Earn rewards (typically 1% of debt) for their service
* Compete to execute liquidations quickly and efficiently

**Why This Benefits You**

* **Faster Response**: Liquidations happen within blocks, not hours
* **Fair Execution**: Open competition prevents insider advantages
* **Lower Losses**: Quick action minimizes how underwater positions get
* **Always Active**: Global network ensures 24/7 coverage

Anyone can be a keeper — no special permissions needed. This open system ensures liquidations happen promptly and fairly, protecting both borrowers and the protocol.

## Why Ripe's System is Superior

### Quick Comparison

| Feature                | Traditional DeFi       | Ripe Protocol                |
| ---------------------- | ---------------------- | ---------------------------- |
| **Liquidation Amount** | Entire position (100%) | Only what's needed (partial) |
| **Warning System**     | None                   | Deleverage + Redemption zones |
| **Liquidation Fee**    | 13-50% penalty         | 5-15% fixed discount         |
| **Who Can Buy**        | MEV bots only          | Anyone (pools + auctions)    |
| **Proactive Options**  | None                   | Deleverage with zero penalty |
| **Market Impact**      | Large dumps            | Phased, orderly process      |

### Borrower Protection Features

1. **Proactive Deleveraging**: Reduce debt before liquidation with zero fees
2. **Graduated Intervention**: Redemption buffer before liquidation
3. **Partial Liquidations**: Only liquidate what's necessary
4. **Fixed Discounts**: No arbitrary penalties or excessive fees
5. **Multiple Mechanisms**: Two phases provide redundancy

### System Stability Benefits

1. **Orderly Process**: Phased approach prevents cascade failures
2. **Deep Liquidity**: Stability pools provide instant buyers
3. **Market Independence**: Not reliant on external exchange depth
4. **Rapid Execution**: Automated systems respond instantly
5. **Proven Resilience**: Designed for extreme market conditions

## What If Bad Debt Occurs?

Despite all protective mechanisms, extreme market conditions could potentially create bad debt (where liquidation proceeds don't fully cover the debt). The protocol has a clear resolution mechanism:

**Bond Sales for Recovery**: The protocol can sell [bonds](../governance-and-economics/10-bonds.md) to raise funds that clear bad debt. This process:

* Creates RIPE tokens beyond the 1 billion supply cap
* Distributes the dilution fairly across all RIPE holders
* Ensures bond buyers receive their full allocation
* Maintains protocol solvency without penalizing users

For example, if 1M RIPE is needed to cover bad debt, the total supply becomes 1.001 billion. This transparent mechanism ensures GREEN remains fully backed while distributing any dilution proportionally across all RIPE holders.

## Liquidations That Don't Ruin Lives

Here's what actually matters:

**When others get liquidated**: They lose everything. Position gone. Starting over from zero.

**When you get liquidated on Ripe**: You lose some. Position survives. Still in the game.

Even better: you can [deleverage](05-deleverage.md) before liquidation even happens — using your GREEN, sGREEN, or stablecoins to pay down debt with zero penalties. That's not an option on other protocols.

That $312.50 difference in our example? That's rent money. That's staying in crypto versus rage quitting. That's the difference between a setback and a catastrophe.

The protocol doesn't do this to be nice. It does it because borrowers who survive keep borrowing, keep paying interest, keep the system running. Your success is the protocol's success.

So go ahead. Take that loan. You've got deleverage, redemption buffers, stability pools, and auctions watching your back.

***

_For technical implementation details, see the_ [_AuctionHouse Technical Documentation_](https://ripe-finance.gitbook.io/ripe-developers/core-lending/auctionhouse)_._
