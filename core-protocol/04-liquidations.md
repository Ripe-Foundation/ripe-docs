---
description: When Leverage Goes Wrong (But Not Too Wrong)
---

# Liquidations: When Leverage Goes Wrong (But Not Too Wrong)

When an eligible liquidation call is submitted for a position beyond its liquidation threshold, Ripe calculates a repayment target designed to restore debt health rather than automatically closing the entire account.

That makes many liquidations partial, but **partial is not guaranteed**. If the account is deeply underwater, has too little eligible collateral, or must repay all remaining debt to recover, liquidation can consume all eligible collateral and clear the full debt.

[Deleverage](05-deleverage.md), credit redemption, and liquidation are separate mechanisms. Deleverage and redemption may reduce risk before liquidation; they are not automatic phases inside a liquidation call.

## Executive Summary

**Key Points:**

* 🛡️ **Separate risk tools**: Deleverage and redemption can act before liquidation
* 📊 **Target-based**: Liquidation aims to restore health, but the target can be the full debt
* 💰 **Episode-based fees**: Liquidation and keeper fees are assessed once for a liquidation episode, not on every retry
* ⚡ **Permissionless execution**: Eligible callers can submit liquidation transactions; monitoring coverage and timing are not guaranteed
* 🎯 **Two settlement routes**: Eligible collateral tries Stability liquidity first, then uses an auction when necessary

**Quick Visual: The Liquidation Flow**

```
Optional, separate actions before liquidation
├─ Deleverage eligible assets to reduce debt
└─ Credit redemption when its own conditions are met
                      ↓
Account reaches its liquidation threshold
                      ↓
Start one liquidation episode
├─ Calculate the health-restoring repayment target
└─ Assess liquidation and keeper fees once
                      ↓
Traverse configured priority collateral, then other vault assets
                      ↓
For each eligible asset
├─ Try compatible Stability liquidity with available capacity
└─ Save the remainder for a Dutch auction when configured
                      ↓
Restore debt health, or retry after outstanding auctions are resolved
```

**What This Means For You:**

* ✅ Eligible proactive deleverage may reduce or avoid liquidation risk
* ✅ A target-based liquidation may preserve collateral when the account has enough value
* ✅ The episode's configured base and keeper fees are assessed once rather than on every retry
* ✅ Stability settlement and target-based auctions provide bounded, transparent execution paths

## Quick Navigation

**Understanding the Basics:**

* [Why Liquidations Matter](04-liquidations.md#why-liquidations-matter) - Protocol safety and borrower protection
* [Risk Zones](04-liquidations.md#understanding-the-risk-zones) - Three thresholds and visual guide
* [Deleverage First](05-deleverage.md) - Proactive debt reduction (separate page)
* [Redemption Buffer](04-liquidations.md#the-redemption-buffer) - A separate pre-liquidation mechanism

**The Liquidation Process:**

* [Stability Pool Settlement](04-liquidations.md#stability-pool-settlement) - Conditional in-protocol liquidity
* [Dutch Auctions](04-liquidations.md#dutch-auctions) - Fallback with time-based discounts

**Advanced Topics:**

* [Liquidation Economics](04-liquidations.md#liquidation-economics) - Fees and calculations
* [Keeper Network](04-liquidations.md#the-keeper-network) - Permissionless, caller-submitted execution
* [Bad Debt Handling](04-liquidations.md#what-if-bad-debt-occurs) - Last resort measures

***

## Why Liquidations Matter

### Protecting Protocol Solvency

Liquidations are a critical solvency mechanism for [GREEN](01-green-stablecoin.md). When [borrowing positions](02-borrowing.md) become undercollateralized because collateral values fall or interest accrues, liquidation attempts to repay debt from eligible collateral and limit bad-debt accumulation. Execution outcomes still depend on collateral value, settlement liquidity, auction demand, and caller activity.

### The Borrower-Friendly Approach

Ripe calculates a repayment target from the account's live debt, collateral value, and configured target LTV. When the account has enough eligible collateral, this can preserve more of the position than an automatic full close. The target can still equal the full debt, and execution may consume all eligible collateral. This approach:

* **Targets Recovery**: Process collateral only until the target is met or eligible collateral is exhausted
* **Reduces Market Impact**: Smaller liquidations mean less selling pressure
* **Enables Recovery**: A partial result can allow the remaining position to recover
* **Avoids Repeat Charges**: Retries in the same liquidation episode do not assess the episode fees again

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
* Varies by asset and deployment configuration
* Example: At an assumed 77% threshold, others can exchange GREEN for eligible collateral at its oracle value

**3. Liquidation Threshold**

* The danger zone where a position becomes eligible for forced liquidation
* Calculated as minimum collateral needed for your debt
* Varies by asset and deployment configuration
* Example: At an assumed 80% threshold with $8,000 debt, the position becomes eligible when collateral is at or below $10,000

### How Risk Escalates: Visual Zone Map

Consider an illustrative position with $10,000 initial collateral and $6,000 debt, using assumed 70%, 77%, and 80% thresholds:

```
POSITION HEALTH VISUALIZATION (for $6,000 debt, ETH/BTC collateral)
←─────────────────────────────────────────────────────────────→
$10,000                    $8,571      $7,792     $7,500      $0
  YOU                        ↓           ↓           ↓
  ARE                   Max LTV     Redemption  Liquidation
  HERE                   (70%)        (77%)        (80%)

[════ SAFE ZONE ════][CAUTION][REDEMPTION][LIQUIDATION]
     ✅ Healthy        ⚠️ Warning  🚨 Danger    💀 Critical
```

**Zone Breakdown:**

**🟢 Zone 1: Healthy** (Collateral > $8,571)

* Below 70% LTV maximum
* Can still borrow more
* No forced redemption or liquidation; the owner or a trusted caller can still use proactive deleverage

**🟡 Zone 2: Warning** (Collateral $8,571 - $7,792)

* Exceeded max LTV, cannot borrow
* Still safe from redemption
* Time to add collateral or repay

**🟠 Zone 3: Redemption** (Collateral at or below approximately $7,792 but above $7,500)

* Redemption threshold breached (77%)
* GREEN holders can redeem your collateral
* Can reduce debt through a route separate from liquidation and deleverage

**🔴 Zone 4: Liquidation** (Collateral at or below $7,500)

* Liquidation threshold breached (80%)
* The position is eligible for a liquidation episode
* Active liquidation begins only when an eligible transaction starts the episode

*Note: These values illustrate how the zones relate; see [RIPE Params](https://params.ripe.finance) for live terms.*

_For a detailed explanation of how these thresholds work together, see_ [_Understanding Three Thresholds_](02-borrowing.md#how-thresholds-work-together-a-visual-guide) _in the borrowing documentation._

## The Redemption Buffer

### A Separate Pre-Liquidation Mechanism

Credit redemption has its own eligibility checks, pricing, asset traversal, and settlement. When its configured conditions are met, a GREEN holder can exchange GREEN for eligible collateral and reduce the selected borrower's debt.

A redemption can improve debt health and may prevent a later liquidation, but it is **not** the first phase of `liquidateUser`. Likewise, [deleverage](05-deleverage.md) is a separate debt-reduction route that can consume only assets eligible for that mechanism.

## The Liquidation Process

When liquidation becomes necessary, Ripe opens a liquidation episode and attempts to repay a calculated target using eligible collateral.

### Before Liquidation: Deleverage

Before your position reaches liquidation, you may have options to reduce debt without liquidation fees:

* **[Deleverage](05-deleverage.md)** with assets configured for debt repayment
* Burn GREEN-side assets or transfer eligible stable-side collateral for credit against debt
* Use an owner-approved asset order, or let the broad route follow protocol priority

This is handled by the separate [Deleverage](05-deleverage.md) system. The owner, trusted protocol callers, and delegates with borrowing permission have broader control; an untrusted caller is limited to a bounded deleverage when the account is in the configured danger zone.

**Key Point**: The liquidation path skips assets configured for stable-side burn or transfer so that Deleverage can handle them. Liquidation processes other eligible collateral through Stability settlement and/or auction according to each asset's configuration.

### What Happens When Liquidation Starts

When an eligible liquidation call is submitted after the position crosses the liquidation threshold:

1. **Position enters `inLiquidation` state**
   * You are blocked from taking new borrows
   * You can still repay debt to exit liquidation
   * Eligible collateral becomes available to the liquidation process

2. **The episode's fees are calculated once**
   * The base liquidation fee and bounded keeper fee are assessed only when the account first enters the episode
   * A later retry while `inLiquidation` does not assess those fees again
   * A first pass that repays nothing and starts no auction is economically inert and does not earn a keeper fee

3. **A repayment target and traversal order are set**
   * The target uses live debt and account terms and aims below the danger threshold
   * It is often less than the full debt, but can equal the full debt
   * Configured priority assets are visited first, followed by the user's other vault assets

4. **Each eligible asset chooses a settlement route**
   * The protocol first checks compatible Stability vaults when that asset permits Stability settlement
   * Any configured auction-eligible remainder can be saved for a Dutch auction

5. **The episode resolves or waits**
   * Debt health restoration clears the account's liquidation restriction
   * An outstanding auction owns the current liquidation pass, so another liquidation pass waits until auctions are bought, removed, or restarted as appropriate
   * If the account remains unhealthy after auctions are resolved, the protocol can retry without charging the episode fees again

### Stability Pool Settlement

For an asset configured to use [Stability pools](../earning-and-rewards/02-stability-pools.md), the AuctionHouse checks each permitted Stability vault before moving collateral.

**How Pool Swaps Work**

1. The Stability vault must support the settlement asset and be able to accept the collateral as a claim asset
2. It must have spendable, unreserved settlement liquidity and a usable price
3. The vault gives up settlement liquidity and receives the borrower's collateral as claimable custody
4. The debt receives credit for the value actually supplied by the Stability vault

**The Win-Win Dynamic**

* Compatible in-protocol liquidity can reduce how much collateral reaches auction
* The configured liquidation spread gives the Stability cohort more collateral value than the settlement value it supplies
* Market-price changes, claim liquidity, and pool capacity still affect the depositor's realized outcome

**Additional Pool Benefits**

* Depositors may earn configured RIPE rewards while their Stability-vault shares remain deposited
* Claimable collateral remains part of share NAV while it is active and priced
* Claiming collateral and withdrawing the original settlement asset are separate actions

**Special Note on Permissioned Assets** For regulated assets (tokenized securities, real estate):

* Dedicated permissioned pools with whitelisted participants
* Same swap mechanics but restricted access
* Ensures compliance throughout liquidation process

_For deeper understanding of stability pool mechanics, see_ [_Stability Pools_](../earning-and-rewards/02-stability-pools.md)_._

If the vault is incompatible, full, paused, reserved, unpriced, or out of spendable liquidity, the liquidation skips it. When the collateral is configured for auction, the unsatisfied portion falls back to the Dutch-auction path.

### Dutch Auctions

For auction-eligible collateral not fully handled by Stability settlement:

**Time-Based Discounts**

* Auctions begin after their configured delay
* Discounts increase linearly from the configured start discount to the maximum discount
* A caller with GREEN can buy when global and asset purchase controls are enabled and the recipient passes any configured allowlist
* Purchases settle immediately during the open purchase window

**Auction Mechanics**

* A purchase is bounded by the buyer's requested amount, available GREEN, available collateral, and the borrower's **live debt**
* GREEN payment is burned, reducing your debt
* Only the GREEN corresponding to collateral actually delivered is spent; unused input is refunded after the batch
* The maximum discount applies on the final purchasable block. The recorded end block is exclusive and cannot be purchased
* An auction is removed when its collateral is depleted. An active auction that reaches its end block can be explicitly removed as expired before later processing or restart

## Liquidation Economics

### Understanding Liquidation Fees

Liquidation economics are configurable rather than one fixed protocol-wide percentage:

* The account's weighted liquidation fee establishes the base fee for a new liquidation episode
* The Stability-settlement spread lets a participating cohort receive collateral at a discount; it is not necessarily identical to an additive fee charged after settlement
* The keeper fee is bounded by protocol minimums, maximums, and available collateral surplus
* Stability settlement can cover the nominal base fee through its collateral spread. Any unpaid base fee and keeper fee are added to the account's debt before repayment is recorded
* The same account may need multiple calls or auctions, but these episode fees are not assessed again while it remains in liquidation

### Target-Based Liquidation Design

Ripe aims to liquidate enough to restore healthy LTV:

**Target Calculation**

* Uses current debt, total collateral value, and the account's lowest applicable LTV
* Applies the configured payback buffer to choose a safer target
* Stops traversal when that target has been supplied or no further eligible collateral can be processed
* Caps creditable repayment so settlement cannot repay more than the debt that can actually be cleared

> **💡 TL;DR**: “Target-based” means Ripe does not start by assuming a full close. It does **not** promise a partial result. The repayment target can be the full debt, and a distressed account can lose all eligible collateral.

## The Keeper Network

### Permissionless Liquidation Callers

Keepers are independent operators that can monitor positions and submit liquidation transactions. The contracts make liquidation permissionless; they do not promise that a keeper will observe or execute every opportunity within a particular time.

**How Keepers Work**

* Monitor positions for liquidation eligibility
* Submit liquidations once positions are unsafe
* Earn a configured, bounded reward when a productive call starts a new liquidation episode
* Compete to execute liquidations quickly and efficiently

**Why This Benefits You**

* **Open Access**: Any eligible caller can compete to submit a liquidation
* **Economic Incentive**: A productive first call can earn the configured keeper reward
* **Risk Reduction**: Earlier execution can reduce how far a position moves underwater
* **Permissionless Participation**: Any eligible caller can monitor accounts and submit a liquidation transaction

No keeper allowlist is required for the liquidation entry point. Actual execution speed depends on offchain monitoring, network conditions, and transaction inclusion.

## Why Ripe's System is Superior

### Quick Comparison

| Feature                | Traditional DeFi       | Ripe Protocol                |
| ---------------------- | ---------------------- | ---------------------------- |
| **Liquidation Amount** | Often all-or-nothing | Health-restoring target, which can be the full debt |
| **Warning System**     | None                   | Deleverage + Redemption zones |
| **Liquidation Fee**    | Protocol dependent     | Configurable episode fees and settlement discounts |
| **Who Can Buy**        | Often specialist bots  | Eligible Stability vaults and permitted auction buyers |
| **Proactive Options**  | None                   | Deleverage with zero penalty |
| **Market Impact**      | External sale pressure | Stability settlement where available, auction fallback |

### Borrower Protection Features

1. **Proactive Deleveraging**: Reduce debt before liquidation with zero fees
2. **Graduated Intervention**: Redemption buffer before liquidation
3. **Target-Based Liquidations**: Seek debt health instead of assuming a full close
4. **One Fee Assessment Per Episode**: Retry passes do not repeatedly assess liquidation fees
5. **Multiple Settlement Routes**: Stability eligibility plus auction fallback provide redundancy

### System Stability Benefits

1. **Layered Tools**: Redemption, deleverage, liquidation, Stability settlement, and auctions are distinct mechanisms with separate conditions
2. **Conditional Liquidity**: Stability vaults can settle compatible collateral while capacity is available
3. **Public Fallback**: Auctions provide a second route when Stability settlement is unavailable or incomplete
4. **Permissionless Execution**: Independent callers can submit eligible liquidations
5. **Stress Design**: The settlement paths are designed to operate under extreme market conditions, subject to available liquidity and demand

## What If Bad Debt Occurs?

Despite all protective mechanisms, extreme market conditions could potentially create bad debt (where liquidation proceeds don't fully cover the debt). The protocol has a clear resolution mechanism:

**Bond Sales for Recovery**: A qualifying [bond](../governance-and-economics/03-bonds.md) purchase can credit the oracle-valued payment against outstanding bad debt. In this process:

* The complete computed RIPE payout must first fit the available bond allowance
* The payout associated with cleared bad debt is accounted outside ordinary bond-distribution usage
* That accounting can expand minted supply beyond RIPE's 1 billion-token base allocation and dilute holders
* Purchase enablement, asset, epoch, capacity, pricing, and minting controls still apply

Bond recovery is a conditional accounting path, not a guarantee that all bad debt will be cleared or that GREEN will remain fully backed in every market outcome.

## Liquidations That Don't Ruin Lives

Here's what actually matters:

**When an account is liquidated on Ripe**: The protocol targets debt health and stops when the target is met. A well-collateralized account may retain collateral; a severely distressed account may not.

Even better: you can [deleverage](05-deleverage.md) before liquidation — using eligible assets to reduce debt without liquidation fees.

The useful protection is the mechanism's target, not a guarantee of what remains. Monitor debt health, understand which collateral is eligible for each route, and use repay or deleverage before the account reaches liquidation.

***

_For technical implementation details, see the_ [_AuctionHouse Technical Documentation_](https://ripe-finance.gitbook.io/ripe-developers/core-lending/auctionhouse)_._
