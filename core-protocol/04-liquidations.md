---
description: When Leverage Goes Wrong (But Not Too Wrong)
---

# Liquidations: When Leverage Goes Wrong (But Not Too Wrong)

When an eligible liquidation call is submitted for a position that has reached or crossed its liquidation threshold, Ripe calculates a repayment target designed to restore debt health rather than automatically closing the entire account.

That means a liquidation **may be partial**, but partial repayment is not guaranteed. If the account is deeply underwater or has too little eligible collateral, liquidation can consume all eligible collateral without clearing all of the debt. If full repayment is both required and available, it can instead clear the entire debt.

[Deleverage](05-deleverage.md), credit redemption, and liquidation are separate mechanisms. Deleverage and redemption may reduce risk before liquidation; they are not automatic phases inside a liquidation call.

The borrower address also matters. AuctionHouse liquidation and ordinary credit redemption skip an address registered as an Underscore Earn vault. That exception belongs to the recognized borrower, not to every user who happens to deposit an Underscore share token as collateral.

## Executive Summary

**Key Points:**

* 🛡️ **Separate risk tools**: Deleverage and redemption can act before liquidation
* 📊 **Target-based**: Liquidation aims to restore health, but the target can be the full debt
* 💰 **Episode-based fees**: Liquidation and keeper fees are assessed once for a liquidation episode, not on every retry
* ⚡ **Permissionless execution**: Eligible callers can submit liquidation transactions; monitoring coverage and timing are not guaranteed
* 🎯 **Two settlement routes**: Eligible collateral tries Stability liquidity first, then uses a configured auction fallback when available

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
* Example: At an assumed 77% threshold, others can exchange GREEN for eligible collateral using an oracle-derived request quote; partial vault delivery scales the debt credit proportionally, subject to integer rounding

**3. Liquidation Threshold**

* The danger zone where a position becomes eligible for forced liquidation
* Calculated as minimum collateral needed for your debt
* Varies by asset and deployment configuration
* Example: At an assumed 80% threshold with $8,000 debt, the position becomes eligible when collateral is at or below $10,000

### How Risk Escalates: Visual Zone Map

Consider an illustrative position with $10,000 initial collateral and $6,000 debt, using assumed 70%, 77%, and 80% thresholds:

```
POSITION HEALTH VISUALIZATION (for $6,000 debt, illustrative WETH/WBTC collateral)
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

**🟡 Zone 2: Warning** (Collateral at or below approximately $8,571 but above approximately $7,792)

* Maximum LTV reached or exceeded, so no further borrowing is available
* At the maximum boundary, borrowing-power collateral has no withdrawal room; above it, whole-account housekeeping blocks ordinary withdrawals, including zero-LTV assets
* Still above the redemption boundary
* Repay to return debt to current max-borrow capacity; adding collateral can also help when the ordinary deposit checks pass

**🟠 Zone 3: Redemption** (Collateral at or below approximately $7,792 but above $7,500)

* Redemption threshold reached (77%)
* GREEN holders can redeem your collateral
* Can reduce debt through a route separate from liquidation and deleverage

**🔴 Zone 4: Liquidation** (Collateral at or below $7,500)

* Liquidation threshold reached (80%)
* The position is eligible for a liquidation episode
* Active liquidation begins only when an eligible transaction starts the episode

*Note: These values illustrate how the zones relate; see [RIPE Params](https://params.ripe.finance) for live terms.*

_For a detailed explanation of how these thresholds work together, see_ [_Understanding Three Thresholds_](02-borrowing.md#how-thresholds-work-together-a-visual-guide) _in the borrowing documentation._

## The Redemption Buffer

### A Separate Pre-Liquidation Mechanism

Credit redemption has its own eligibility checks, pricing, asset traversal, and settlement. When its configured conditions are met, a GREEN holder can exchange GREEN for eligible collateral and reduce the selected borrower's debt. Reaching the liquidation threshold does not by itself set `inLiquidation`, so redemption may remain available until an eligible liquidation transaction actually starts the episode.

A redemption can improve debt health and may prevent a later liquidation, but it is **not** the first phase of `liquidateUser`. Likewise, [deleverage](05-deleverage.md) is a separate debt-reduction route that can consume only assets eligible for that mechanism.

## The Liquidation Process

When liquidation becomes necessary, Ripe opens a liquidation episode and attempts to repay a calculated target using eligible collateral.

### Before Liquidation: Deleverage

Before your position reaches liquidation, you may have options to reduce debt without liquidation fees:

* **[Deleverage](05-deleverage.md)** with assets configured for debt repayment
* Burn GREEN-side assets or transfer eligible stable-side collateral for credit against debt
* Use an owner-approved asset order, or let the broad route follow protocol priority

This is handled by the separate [Deleverage](05-deleverage.md) system. Caller authority depends on the deleverage entry point: the ordered-specific route recognizes the owner and permitted delegates, while an ordinary self-call through the broad route remains subject to its untrusted health cap.

**Key Point**: The liquidation path skips GREEN or sGREEN marked to burn as payment, and it skips assets marked for transfer to Endaoment, so that Deleverage can handle them. Liquidation processes other eligible collateral through Stability settlement and/or auction according to each asset's configuration.

### What Happens When Liquidation Starts

When an eligible liquidation call is submitted once the position reaches or crosses the liquidation threshold:

1. **Position enters `inLiquidation` state**
   * You are blocked from taking new borrows
   * All user withdrawals are blocked while debt remains and `inLiquidation` is set, including withdrawals of assets that do not contribute borrowing power
   * You can still repay debt; the restriction clears when debt is zero or no greater than the account's current maximum borrowing capacity, not merely when the position moves back above the liquidation threshold
   * Eligible collateral becomes available to the liquidation process

2. **The episode's fees are calculated once**
   * The base liquidation fee and bounded keeper fee are assessed only when the account first enters the episode
   * A nonzero keeper reward is newly minted as GREEN, or optionally deposited into sGREEN for the keeper, while the corresponding keeper fee is added to the borrower's debt
   * A later retry while `inLiquidation` does not assess those fees again
   * A first pass that repays nothing and starts no auction is economically inert and does not earn a keeper fee

3. **A repayment target and traversal order are set**
   * The target uses live debt and account terms and aims below the danger threshold
   * It can be less than the full debt, but can also equal the full debt
   * Configured priority assets are visited first, followed by the user's other vault assets

4. **Each eligible asset chooses a settlement route**
   * The protocol first checks compatible Stability vaults when that asset permits Stability settlement
   * Any remainder whose asset is configured for auction can be saved for a Dutch auction

5. **The episode resolves or waits**
   * Debt at or below current maximum borrowing capacity clears the account's liquidation restriction
   * An outstanding auction owns the current liquidation pass, so another liquidation pass waits until auctions are bought, removed, or restarted as appropriate
   * A later valuation quarantine does not automatically pause or remove an auction that already exists; that auction remains governed by its own active status, pricing, and purchase checks
   * If the account remains unhealthy after auctions are resolved, the protocol can retry without charging the episode fees again

### Stability Pool Settlement

For an asset configured to use [Stability pools](../earning-and-rewards/02-stability-pools.md), the AuctionHouse checks each permitted Stability vault before moving collateral.

**How Pool Swaps Work**

1. The Stability vault must support the settlement asset and be able to accept the collateral as a claim asset
2. The liquidation consumes any claimable GREEN associated with that settlement asset first
3. If more repayment is needed, the vault must have spendable, unreserved settlement-asset liquidity and a usable price
4. GREEN is burned; sGREEN is redeemed and its GREEN is burned; any other settlement asset is transferred to EndaomentFunds
5. The vault receives the borrower's collateral as claimable custody. Debt credit uses the route's settlement-value quote scaled to the amount the vault actually supplies, subject to integer rounding

**The Win-Win Dynamic**

* Compatible in-protocol liquidity can reduce how much collateral reaches auction
* A nonzero effective episode spread, after the episode fee caps, gives the Stability cohort more quoted collateral value than the settlement value it supplies
* Market-price changes, claim liquidity, and pool capacity still affect the depositor's realized outcome

**Additional Pool Benefits**

* Depositors may earn configured RIPE rewards while their Stability-vault shares remain deposited
* Claimable collateral remains part of share NAV while it is active and priced
* Claiming collateral and withdrawing the original settlement asset are separate actions

**Optional recipient controls**

A claim asset can independently configure a recipient allowlist and a special Stability-vault route. A claim checks its recipient against the claim asset's allowlist; the routing choice and recipient control are separate, and neither guarantees regulatory compliance.

_For deeper understanding of stability pool mechanics, see_ [_Stability Pools_](../earning-and-rewards/02-stability-pools.md)_._

If the vault is incompatible, full, paused, reserved, unpriced, or out of spendable liquidity, the liquidation skips it. When the collateral is configured for auction, the unsatisfied portion falls back to the Dutch-auction path.

### Dutch Auctions

For auction-eligible collateral not fully handled by Stability settlement:

**Time-Based Discounts**

* Auctions begin after their configured delay
* Discounts increase linearly from the configured start discount to the maximum discount
* A caller with GREEN can buy when global and asset purchase controls are enabled and the recipient passes any configured allowlist. The borrower cannot be the recipient; when caller and recipient differ, the caller also needs the account-wide third-party deposit permission or recognized Underscore-wallet ownership
* Purchases settle immediately during the open purchase window

**Auction Mechanics**

* A purchase is bounded by the buyer's requested amount, available GREEN, available collateral, and the borrower's **live debt**
* GREEN payment is burned, reducing the borrower's debt
* Only the GREEN corresponding to the collateral amount credited by the vault is spent; unused input is refunded after the batch
* The maximum discount applies on the final purchasable block. The recorded end block is exclusive and cannot be purchased
* An auction is removed when its collateral is depleted. An active auction that reaches its end block can be explicitly removed as expired before later processing or restart

## Liquidation Economics

### Understanding Liquidation Fees

Liquidation economics are configurable rather than one fixed protocol-wide percentage:

* The account's weighted liquidation fee establishes the base fee for a new liquidation episode
* Episode base and keeper fees are capped by available collateral surplus and a debt-relative hard ceiling. The Stability discount ratio uses the resulting base fee; only that nominal base fee can be treated as spread-paid, while the keeper fee remains debt
* The Stability-settlement spread lets a participating cohort receive collateral at a discount; it is not necessarily identical to an additive fee charged after settlement
* The keeper fee is bounded by protocol minimums, maximums, available collateral surplus, and the combined debt-relative fee ceiling; when nonzero, the keeper reward is minted as GREEN or optionally delivered through sGREEN
* Stability settlement can cover the nominal base fee through its collateral spread. Any unpaid base fee and keeper fee are added to the account's debt before repayment is recorded
* The same account may need multiple calls or auctions, but these episode fees are not assessed again while it remains in liquidation

### Target-Based Liquidation Design

Ripe aims to liquidate enough to restore healthy LTV:

**Target Calculation**

* Uses current debt, total collateral value, and the account's lowest applicable LTV
* Applies the configured payback buffer to choose a safer target
* Stops traversal when that target has been supplied or no further eligible collateral can be processed
* Caps creditable repayment so settlement cannot repay more than the debt that can actually be cleared

> **💡 TL;DR**: "Target-based" means Ripe does not start by assuming a full close. It does **not** promise a partial result. The repayment target can be the full debt, and a distressed account can lose all eligible collateral.

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

## How Ripe's Resolution Routes Fit Together

### Borrower Protection Features

1. **Proactive Deleveraging**: Reduce debt before liquidation with zero fees
2. **Graduated Intervention**: Redemption buffer before liquidation
3. **Target-Based Liquidations**: Seek debt health instead of assuming a full close
4. **One Fee Assessment Per Episode**: Retry passes do not repeatedly assess liquidation fees
5. **Multiple Settlement Routes**: Stability eligibility plus auction fallback provide redundancy

### System Stability Benefits

1. **Layered Tools**: Redemption, deleverage, liquidation, Stability settlement, and auctions are distinct mechanisms with separate conditions
2. **Conditional Liquidity**: Stability vaults can settle compatible collateral while capacity is available
3. **Configured Fallback**: Auction-enabled assets have a second route when Stability settlement is unavailable or incomplete
4. **Permissionless Execution**: Independent callers can submit eligible liquidations
5. **Conditional Operation**: Every route still depends on its pricing, custody, liquidity, configuration, and caller conditions

## What If Bad Debt Occurs?

Despite all protective mechanisms, extreme market conditions could potentially create bad debt (where liquidation proceeds don't fully cover the debt). The protocol has a clear resolution mechanism:

**Bond Sales for Recovery**: A qualifying [bond](../governance-and-economics/03-bonds.md) purchase can credit the oracle-valued payment against outstanding bad debt. In this process:

* The complete computed RIPE payout must first fit the available bond allowance
* The payout associated with cleared bad debt is accounted outside ordinary bond-distribution usage
* Authorized cap administration must count that separately accounted payout toward RIPE's protocol-wide 1 billion-token cap across all blockchains
* Purchase enablement, asset, epoch, capacity, pricing, and minting controls still apply

Bond recovery is a conditional accounting path, not a guarantee that all bad debt will be cleared or that GREEN will remain fully backed in every market outcome.

## Liquidations That Don't Ruin Lives

Here's what actually matters:

**When an account is liquidated on Ripe**: The protocol targets debt health and stops when the target is met. A well-collateralized account may retain collateral; a severely distressed account may not.

Even better: you can [deleverage](05-deleverage.md) before liquidation — using eligible assets to reduce debt without liquidation fees.

The useful protection is the mechanism's target, not a guarantee of what remains. Monitor debt health, understand which collateral is eligible for each route, and use repay or deleverage before the account reaches liquidation.
