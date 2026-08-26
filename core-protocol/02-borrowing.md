---
description: Borrow GREEN against supported Stock Tokens and eligible portfolio collateral.
---

# Borrowing Against Stock Tokens and Your Portfolio

Stock Tokens can represent meaningful market exposure, but traditional lending often requires selling that exposure or managing a separate position for each asset.

Ripe lets supported [Stock Tokens](03-collateral-assets.md#stock-tokens-as-collateral) with configured borrowing power back a [GREEN](01-green-stablecoin.md) loan without a protocol sale at origination. They can work alongside other eligible non-Stability collateral in one position, with each asset contributing according to its independently configured terms.

One portfolio-backed position. Weighted terms. Liquidity without first giving up the deposited token exposure.

## How Borrowing Works

### Unified Position Structure

In Ripe Protocol, eligible collateral across non-Stability vaults backs a single, unified loan position. You don't have a separate loan for each Stock Token or other collateral asset — supported collateral with nonzero borrowing terms supports one consolidated debt position. Positions in vaults classified as Stability vaults are excluded from borrowing collateral terms, even though they can serve other protocol roles.

### The Credit Calculation Engine

Ripe's credit engine performs sophisticated calculations to determine your borrowing capacity:

1. **Collateral Valuation**: Each deposited asset is valued using the first usable configured price source
2. **LTV Application**: Asset-specific loan-to-value ratios determine borrowing power
3. **Term Weighting**: Multiple assets create weighted average terms
4. **Risk Assessment**: Dynamic adjustments based on market conditions

This multi-factor approach ensures fair credit access while maintaining system security.

### Weighted Debt Terms Explained

When you deposit multiple collateral types, Ripe doesn't just average your terms — it weights them based on each asset's contribution to your total borrowing power. Here's how it works:

**Hypothetical Stock-Led Portfolio:**

```
Stock Token A: $10,000 value × 60% LTV = $6,000 borrowing power
WETH: $5,000 value × 70% LTV = $3,500 borrowing power
Stable Asset: $10,000 value × 80% LTV = $8,000 borrowing power

Total Borrowing Power = $17,500
```

**Weighted Interest Rate Calculation:**

```
Stock Token A weight: $6,000 / $17,500 = 34.3%
WETH weight: $3,500 / $17,500 = 20.0%
Stable Asset weight: $8,000 / $17,500 = 45.7%

If illustrative rates are: Stock Token A 8%, WETH 7%, Stable Asset 5%
Position rate = (8% × 34.3%) + (7% × 20.0%) + (5% × 45.7%) = 6.4%
```

This weighting applies to all debt terms: interest rates, liquidation thresholds, redemption thresholds, and origination fees. Assets with higher borrowing power have proportionally greater influence on your overall terms.

The example is illustrative and does not identify supported assets or live terms. See [RIPE Params](https://params.ripe.finance) for current asset support and debt terms.

## Key Safety Thresholds

### Understanding Your Risk Levels

Three critical thresholds govern your position's safety. Understanding how they work — and how they work together — is essential for managing risk.

All threshold values in this section are hypothetical examples, not live protocol parameters.

### The Three Thresholds Explained

**1. Loan-to-Value (LTV) Ratio: Your Borrowing Limit**

The LTV determines your maximum borrowing capacity as a percentage of collateral value.

* **What it means**: You can borrow up to this percentage of your collateral
* **Direction**: Higher debt OR lower collateral value → Higher LTV usage (risky)
* **Example**: 70% LTV on $10,000 collateral = $7,000 maximum borrow

**2. Redemption Threshold: The Warning Zone**

When your position becomes eligible for collateral redemption by GREEN holders.

* **What it means**: Other users can provide GREEN to reduce your debt and receive oracle-sized eligible collateral
* **How it's calculated**: Position at risk when collateral ≤ debt ÷ redemption threshold
* **Threshold varies by asset**: Stablecoins ~85%, ETH/BTC ~77%, volatile assets ~60%
*   **Example with 85% threshold (stablecoins)**:

    ```
    Your debt: $8,000
    Redemption eligibility begins when collateral ≤ approximately $9,412
    (Because $8,000 ÷ 0.85 = $9,412)
    ```
* **Purpose**: Provides market-based deleveraging and early warning before [liquidation](04-liquidations.md)

**3. Liquidation Threshold: The Danger Zone**

The critical point where the account becomes eligible for forced [liquidation](04-liquidations.md) to protect the protocol.

* **What it means**: Your position becomes eligible for liquidation
* **How it's calculated**: Liquidation when collateral ≤ debt ÷ liquidation threshold
* **Threshold varies by asset**: Stablecoins ~90%, ETH/BTC ~80%, volatile assets ~65%
*   **Example with 90% threshold (stablecoins)**:

    ```
    Your debt: $8,000
    Liquidation eligibility begins when collateral ≤ approximately $8,889
    (Because $8,000 ÷ 0.90 = $8,889)
    ```
* **Execution**: An eligible liquidation still requires a submitted transaction, and the borrower can repay debt

### How Thresholds Work Together: A Visual Guide

Here's a unified view of how all three thresholds create different risk zones. This is a hypothetical ETH/BTC example; exact values are configuration:

```
COLLATERAL VALUE SCALE (for $6,000 debt, ETH/BTC collateral)
←─────────────────────────────────────────────────────────────→
$10,000                    $8,571      $7,792     $7,500      $0

[════ SAFE ZONE ════][CAUTION][REDEMPTION][LIQUIDATION]
     ✅ Healthy        ⚠️ Warning  🚨 Danger    💀 Critical

│                          │            │           │
│                          │            │           └─ Liquidation (80%)
│                          │            │               $6,000 ÷ 0.80 = $7,500
│                          │            │
│                          │            └─ Redemption (77%)
│                          │                $6,000 ÷ 0.77 = $7,792
│                          │
│                          └─ Max Borrow/LTV (70%)
│                              $6,000 ÷ 0.70 = $8,571
│
└─ Your Current Collateral: $10,000
   (167% collateral ratio - very safe!)
```

**Understanding Each Zone:**

**🟢 SAFE ZONE (Below Max LTV / Collateral > $8,571)**

* **Status**: Healthy position with borrowing capacity
* **Actions Available**: Can borrow up to $7,000 total (70% max LTV for ETH/BTC)
* **Risk Level**: Lower, not zero — price, oracle, interest-rate, and execution risks remain
* **What to do**: Normal operations

**🟡 CAUTION ZONE (Between Max LTV and Redemption / Collateral $8,571 - $7,792)**

* **Status**: Over max LTV but still protected
* **Actions Available**: Cannot borrow more. At the maximum boundary, borrowing-power collateral has no withdrawal room; once debt exceeds current max-borrow capacity, ordinary withdrawals revert during whole-account debt housekeeping, including withdrawals of zero-LTV assets. Repay or add collateral.
* **Risk Level**: Medium - approaching danger
* **What to do**: Consider reducing debt or adding collateral

**🟠 REDEMPTION ZONE (Between Redemption and Liquidation / Collateral $7,792 - $7,500)**

* **Status**: Eligible for [redemption](04-liquidations.md#the-redemption-buffer) and [deleveraging](05-deleverage.md)
* **Actions Available**: An eligible GREEN holder can reduce your debt for oracle-sized collateral when the redemption and recipient checks pass, or an eligible caller can [deleverage](05-deleverage.md) your position
* **Risk Level**: High - active intervention needed
* **What to do**: Urgently repay debt, add collateral, or proactively [deleverage](05-deleverage.md)

**🔴 LIQUIDATION ZONE (At or Above Liquidation Threshold / Collateral ≤ $7,500)**

* **Status**: Eligible for [liquidation](04-liquidations.md); a submitted liquidation begins or continues the episode
* **Actions Available**: Repayment remains available subject to its normal controls; liquidation can be retried when no fungible auction remains
* **Risk Level**: Critical - liquidation targets safer account health
* **What to do**: Repay or add collateral urgently; severe shortfalls can produce a full-debt repayment target and exhaust all eligible collateral without fully clearing the debt

**Liquidation episode state**: Once an account is marked `inLiquidation`, a new borrow no longer clears that freeze. All ordinary withdrawals are blocked, including zero-LTV assets. A successful repayment clears the state when debt is zero or no greater than current max-borrow capacity—not merely when the account moves back outside the liquidation threshold—and another liquidation pass can retry when no fungible auction remains.

### The Critical Inverse Relationship

Unlike LTV which calculates forward (debt as % of collateral), redemption and liquidation thresholds work inversely — they define the **minimum collateral required** for a given debt level.

**Quick Reference - Two Ways to View the Same Thresholds (ETH/BTC example):**

| Threshold       | Forward View (LTV)                  | Inverse View (Min Collateral) | Example ($6,000 debt)   |
| --------------- | ----------------------------------- | ----------------------------- | ----------------------- |
| **Max Borrow**  | Can borrow up to 70% of collateral  | Need 143% collateral coverage | Need $8,571+ collateral |
| **Redemption**  | Triggered at 77% debt-to-collateral | Need 130% collateral coverage | Need $7,792+ collateral |
| **Liquidation** | Triggered at 80% debt-to-collateral | Need 125% collateral coverage | Need $7,500+ collateral |

*Note: These figures only illustrate the relationship among thresholds. See [RIPE Params](https://params.ripe.finance) for live terms.*

**What This Means:**

* As debt grows from interest → You approach thresholds
* As collateral value drops → You approach thresholds
* Higher threshold percentages = Tighter requirements = Less room for error

Understanding this inverse relationship helps you monitor the right metrics and take action before it's too late.

## Dynamic Interest Rates

### Base Rates vs Dynamic Adjustments

Your normal interest rate is the weighted average from your collateral mix (as explained in "Weighted Debt Terms" above). A configured dynamic adjustment can apply when sustained reference-pool observations reach the configured danger trigger.

### When Dynamic Rates Activate

Ripe monitors a configured GREEN reference pool as a health indicator. When the current signal is below the trigger, the next eligible debt-term checkpoint stores the weighted base rate; at or above the trigger, it can store a bounded dynamic adjustment. Between checkpoints, existing debt continues accruing at its previously stored rate.

**Key Point**: The signal is built from confirmed intervals, not one spot reading.

**How Pool Monitoring Works:**

1. Snapshots are walked in chronological order.
2. Each valid interval uses the lower GREEN ratio of its two endpoints, so an isolated high observation cannot raise the signal.
3. Valid intervals are weighted by their duration. Excessive gaps are excluded, the latest observation must remain fresh, and no unobserved live tail is extrapolated.
4. Malformed, out-of-order, stale, or overflowing history returns no dynamic signal rather than guessing.

### Rate Adjustment

When an eligible checkpoint observes the duration-weighted ratio at or above the configured danger trigger, the dynamic-rate calculation uses three mechanisms:

1. **Ratio-Based Multiplier**
   * Scales continuously based on pool composition
   * Higher GREEN percentage = Higher multiplier
   * The configured minimum and maximum boosts define the range
2. **Danger Block Accumulation**
   * Danger duration grows only when both endpoints of a valid interval are at or above the trigger
   * The configured per-block increment determines the duration boost
   * Creates urgency for arbitrageurs to restore the peg
   * Mixed safe/danger or stale intervals neither earn nor erase danger duration
3. **Maximum Rate Protection**
   * Hard caps prevent excessive rates regardless of danger block count
   * The configured maximum borrowing rate caps the calculated result

**Rate Reset Behavior**: Recovery is also corroborated. Consecutive safe endpoints must cover the configured freshness window before accumulated danger duration resets.

This mechanism uses protocol block intervals. Converting those intervals to wall-clock time is chain-specific, so the documentation does not hard-code a minutes-per-block estimate.

Interest for an elapsed interval accrues at the borrow rate already stored in the debt record. A borrow, qualifying repayment, or Teller debt-housekeeping update then checkpoints current weighted terms for subsequent accrual; a reference-pool signal change alone does not rewrite dormant debt.

### Underscore Earn Vault Integration

[Underscore Protocol's](https://underscore.finance/) Earn Vaults use two different relationships with Ripe. Core Vault shares can be posted as collateral on Ripe, while Amplified Vaults receive special borrowing terms when they use Ripe inside the vault strategy:

* **Configured discount** on borrowing rates
* **No origination fee** (Daowry waived)
* **No ordinary dynamic-rate branch**: A registered Earn-vault borrower receives its discounted weighted rate rather than the ordinary-borrower dynamic adjustment

AuctionHouse liquidation and ordinary credit redemption skip a borrower address registered as an Underscore Earn vault. This exception follows the borrower address; it does not apply to an ordinary user merely because that user deposited an Earn-vault share as collateral.

**How It Works:**

Underscore Earn Vaults are automated yield strategies. Core Vaults power the base AI-managed yield layer and can be posted as collateral on Ripe. Amplified Vaults go one step further by borrowing GREEN from Ripe inside the strategy to add a second yield layer:

```
Illustrative standard borrowing rate: 6% APR
Illustrative Underscore vault rate: 3% APR (assuming a 50% configured discount)
Origination fee: 0% (waived for Underscore)
```

**For Users:**

You don't interact with Ripe directly for this discount. Instead:

1. Deposit funds into an Underscore Earn Vault
2. Core Vaults run AI-managed yield strategies, while Amplified Vaults add Ripe borrowing inside the strategy
3. The vault — not you — receives the discounted rates when Ripe borrowing is used
4. You benefit from the improved strategy returns

**Scope of the Preferential Terms**

Preferential borrowing terms for registered Underscore Earn vaults are configured inside Ripe. Ripe's contracts do not guarantee how Underscore performance fees are routed or that a RIPE buyback occurs; those outcomes depend on separate Underscore and governance configuration and authorization.

## Borrowing Limits and Safety

### Multi-Tiered Limit System

Ripe implements several limits to ensure sustainable growth:

**1. Collateral-Based Limits**

* Fundamental constraint based on deposited value
* Maximum = Sum of (Asset Value × LTV Ratio)

**2. Per-User Debt Ceiling**

* Configurable individual caps
* Equal limits for all users
* Can be updated by governance

**3. Global Borrower-Debt Limit**

* Limits additional CreditEngine borrowing based on aggregate borrower debt
* Does not cap total GREEN supply or other authorized mint paths
* Can be updated through protocol configuration

**4. Interval Borrowing Limits**

* Configured time-based windows measured in protocol blocks
* Prevents flash loan attacks
* Smooths borrowing demand

**5. Minimum Debt Requirement**

* Configured minimum debt for a new or increased borrowing position
* Governance can update the value
* Ensures position economic viability
* Reduces system complexity

When borrowing, the most restrictive limit applies. This creates a robust framework that protects both individual users and the protocol.

## The Borrowing Experience

### Step-by-Step Process

1. **Deposit Collateral**: Add assets to Ripe vaults
2. **Calculate Capacity**: System determines your borrowing power
3. **Choose Amount**: Borrow up to your available limit
4. **Pay Origination Fee**: A configured one-time fee when enabled
5. **Choose How to Receive**: Receive GREEN, auto-convert to sGREEN, or convert to sGREEN and deposit it into the configured preferred Stability vault

### Distribution Options

When borrowing, you can choose one of three ways to receive your funds:

**Option 1: Direct GREEN**

* Receive standard GREEN stablecoins
* Use immediately for any purpose (swap to USDC)
* Most flexible option

**Option 2: Auto-Convert to** [**sGREEN**](../earning-and-rewards/01-sgreen.md)

* Borrowed GREEN is wrapped into [sGREEN](../earning-and-rewards/01-sgreen.md) when the output exceeds the wrapping floor; an output that does not exceed it remains GREEN
* Receive sGREEN at its current exchange rate; future backing growth depends on configured revenue
* Potential for positive carry (yield > borrow rate)
* No separate conversion transaction needed

**Option 3: Direct to** [**Stability Pool**](../earning-and-rewards/02-stability-pools.md)

* Borrowed GREEN is converted to sGREEN and deposited into the configured preferred [Stability Pool](../earning-and-rewards/02-stability-pools.md) when the output exceeds the wrapping floor; an output that does not exceed it remains GREEN
* Three potential return sources: sGREEN backing growth, liquidation proceeds, and configured [RIPE rewards](../earning-and-rewards/03-ripe-rewards.md)
* Compatible liquidations can produce claimable collateral through the configured spread
* Combines more return sources but is less liquid and carries Stability settlement risk

### Origination Fee (Daowry)

A configured one-time fee on new borrows that:

* Is split between [sGREEN](../earning-and-rewards/01-sgreen.md) backing and a GREEN transfer to governance for potential separately authorized buyback use
* Creates immediate protocol revenue
* Aligns borrower and saver incentives

Illustrative example assuming a 0.25% fee: Borrow 10,000 GREEN → Pay 25 GREEN fee → Receive 9,975 GREEN

## Repayment Flexibility

Ripe Protocol has no contractual maturity schedule or prepayment penalty:

* **No prepayment penalties** - Submit a partial or full repayment without an early-payment fee
* **No scheduled maturity** - Debt remains until repayment or another authorized debt-settlement path reduces it
* **Partial payments allowed** - Reduce debt incrementally
* **Successful debt reduction** - A completed payment immediately reduces the recorded debt

Execution still requires the Teller and CreditEngine repayment route to be available, the account and caller to be permitted, and the payer to have sufficient balance and allowance. A successful payment reduces debt immediately; submitting a transaction does not guarantee execution.

When standard repayment exceeds the live debt, the excess is returned to the payer. Full payoff skips collateral traversal.

## The Future of DeFi Borrowing

Forget the old way. No more portfolio fragmentation. No more wasted collateral. No more choosing between earning yield or accessing liquidity.

With Ripe, supported Stock Tokens can provide access to GREEN without a protocol sale at origination, while other eligible assets can join the same position. Each asset keeps its own configured terms, and those terms combine into one account-level borrowing position.

This is borrowing rebuilt from first principles. One position that actually understands what a portfolio is.

***

_Ready to experience unified borrowing? Your eligible collateral is waiting to work harder._

_For technical implementation details, see the_ [_Credit Engine Technical Documentation_](https://ripe-finance.gitbook.io/ripe-developers/core-lending/creditengine)_._
