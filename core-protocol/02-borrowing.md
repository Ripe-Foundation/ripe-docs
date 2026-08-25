---
description: One Loan, Endless Possibilities.
---

# Borrowing: One Loan, Endless Possibilities

Most lending protocols make you juggle multiple vaults. Different assets, different loans, different liquidation risks. Complexity for no reason.

Ripe combines supported collateral in a single [GREEN](01-green-stablecoin.md) loan. Each asset contributes according to its configured value and terms, producing one position with weighted debt terms.

Finally, borrowing that respects how people actually manage money.

## How Borrowing Works

### Unified Position Structure

In Ripe Protocol, eligible debt-bearing collateral across compatible vaults backs a single, unified loan position. You don't have separate loans for each asset; configured collateral with borrowing power contributes to one consolidated debt position. Stability-vault positions and zero-LTV deposits can have other protocol roles without contributing borrowing power.

### The Credit Calculation Engine

Ripe's credit engine performs sophisticated calculations to determine your borrowing capacity:

1. **Collateral Valuation**: Each deposited asset is valued using the first usable configured price source
2. **LTV Application**: Asset-specific loan-to-value ratios determine borrowing power
3. **Term Weighting**: Multiple assets create weighted average terms
4. **Risk Assessment**: Dynamic adjustments based on market conditions

This multi-factor approach calculates borrowing capacity and weighted terms under the configured risk rules.

### Weighted Debt Terms Explained

When you deposit multiple collateral types, Ripe doesn't just average your terms — it weights them based on each asset's contribution to your total borrowing power. Here's how it works:

**Example Portfolio:**

```
ETH: $10,000 value × 70% LTV = $7,000 borrowing power
WBTC: $5,000 value × 70% LTV = $3,500 borrowing power
USDC: $15,000 value × 80% LTV = $12,000 borrowing power

Total Borrowing Power = $22,500
```

**Weighted Interest Rate Calculation:**

```
ETH weight: $7,000 / $22,500 = 31.1%
WBTC weight: $3,500 / $22,500 = 15.6%
USDC weight: $12,000 / $22,500 = 53.3%

If rates are: ETH 7%, WBTC 7%, USDC 5%
Your rate = (7% × 31.1%) + (7% × 15.6%) + (5% × 53.3%) = 5.9%
```

This weighting applies to all debt terms: interest rates, liquidation thresholds, redemption thresholds, and origination fees. Assets with higher borrowing power have proportionally greater influence on your overall terms.

## Key Safety Thresholds

### Understanding Your Risk Levels

Three critical thresholds govern your position's safety. Understanding how they work — and how they work together — is essential for managing risk.

### The Three Thresholds Explained

**1. Loan-to-Value (LTV) Ratio: Your Borrowing Limit**

The LTV determines your maximum borrowing capacity as a percentage of collateral value.

* **What it means**: You can borrow up to this percentage of your collateral
* **Direction**: Higher debt OR lower collateral value → Higher LTV usage (risky)
* **Example**: 70% LTV on $10,000 collateral = $7,000 maximum borrow

**2. Redemption Threshold: The Warning Zone**

When your position becomes eligible for collateral redemption by GREEN holders.

* **What it means**: Other users can pay off your debt and take equivalent collateral
* **How it's calculated**: Position is eligible when collateral ≤ debt ÷ redemption threshold
* **Threshold varies by asset**: Each supported collateral asset has configured redemption terms
*   **Example with 85% threshold (stablecoins)**:

    ```
    Your debt: $8,000
    Redemption eligibility begins at or below the calculated boundary (~$9,411.76)
    (Because $8,000 ÷ 0.85 = ~$9,411.76)
    ```
* **Purpose**: Provides market-based deleveraging and early warning before [liquidation](04-liquidations.md)

**3. Liquidation Threshold: The Danger Zone**

The critical point where the position becomes eligible for forced [liquidation](04-liquidations.md).

* **What it means**: Your position becomes eligible for liquidation
* **How it's calculated**: Liquidation eligibility begins when collateral ≤ debt ÷ liquidation threshold
* **Threshold varies by asset**: Each supported collateral asset has a configured liquidation threshold
*   **Example with 90% threshold (stablecoins)**:

    ```
    Your debt: $8,000
    Liquidation eligibility begins at or below the calculated boundary (~$8,888.89)
    (Because $8,000 ÷ 0.90 = ~$8,888.89)
    ```
* **Recovery remains possible**: Repayment can restore health, while liquidation can process eligible collateral once triggered

### How Thresholds Work Together: A Visual Guide

Here's a unified view of how all three thresholds create different risk zones. Every value below is illustrative; live terms vary by asset and network:

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
* **Risk Level**: Lower, not zero — collateral, oracle, liquidity, and smart-contract risks remain
* **What to do**: Normal operations

**🟡 CAUTION ZONE (Between Max LTV and Redemption / Collateral $8,571 - $7,792)**

* **Status**: Over max LTV but still protected
* **Actions Available**: Cannot borrow more; can repay/add collateral
* **Risk Level**: Medium - approaching danger
* **What to do**: Consider reducing debt or adding collateral

**🟠 REDEMPTION ZONE (Between Redemption and Liquidation / Collateral $7,792 - $7,500)**

* **Status**: Eligible for [redemption](04-liquidations.md#the-redemption-buffer) and [deleveraging](05-deleverage.md)
* **Actions Available**: Anyone can pay your debt for collateral, or [deleverage](05-deleverage.md) your position
* **Risk Level**: High - active intervention needed
* **What to do**: Urgently repay debt, add collateral, or proactively [deleverage](05-deleverage.md)

**🔴 LIQUIDATION ZONE (At or Above Liquidation Threshold / Collateral ≤ $7,500)**

* **Status**: Eligible for a [liquidation](04-liquidations.md) episode
* **Actions Available**: Repayment remains a recovery path; eligible collateral can be processed by liquidation
* **Risk Level**: Critical - target-based liquidation can reach the full debt
* **What to do**: Repay immediately if possible; do not assume collateral will remain

### The Critical Inverse Relationship

Unlike LTV which calculates forward (debt as % of collateral), redemption and liquidation thresholds work inversely — they define the **minimum collateral required** for a given debt level.

**Quick Reference - Two Ways to View the Same Thresholds (ETH/BTC example):**

| Threshold       | Forward View (LTV)                  | Inverse View (Min Collateral) | Example ($6,000 debt)   |
| --------------- | ----------------------------------- | ----------------------------- | ----------------------- |
| **Max Borrow**  | Can borrow up to 70% of collateral  | Need about 143% collateral coverage | Need at least ~$8,571.43 collateral |
| **Redemption**  | Eligible at or above 77% debt-to-collateral | Need about 130% collateral coverage | Need more than ~$7,792.21 collateral to remain outside the zone |
| **Liquidation** | Eligible at or above 80% debt-to-collateral | Need 125% collateral coverage | Need more than $7,500 collateral to remain outside the zone |

*Note: The figures above demonstrate the threshold relationships; see [RIPE Params](https://params.ripe.finance) for live asset terms.*

**What This Means:**

* As debt grows from interest → You approach thresholds
* As collateral value drops → You approach thresholds
* Higher threshold percentages permit a higher debt-to-collateral ratio before eligibility; the safety buffer depends on the gap between max LTV and the redemption or liquidation threshold

Understanding this inverse relationship helps you monitor the right metrics and take action before it's too late.

## When a Collateral Price Is Unavailable

Ripe does not substitute a cached price when debt-bearing collateral cannot be valued. If an asset that contributes borrowing power has a positive balance but no usable price — or its recorded balance has no usable vault backing — the account enters a valuation quarantine while debt is outstanding.

Quarantine is a safety state:

* The affected collateral contributes no borrowing power
* New borrowing and withdrawals of collateral that supports the debt are blocked
* Liquidation, redemption, and deleveraging are withheld because they cannot be priced safely
* Recovery actions such as repaying debt or adding collateral remain available, subject to normal permissions and pauses

Quarantine is **not** an automatic liquidation or a declaration of insolvency. Once a usable price or backing returns, normal health checks resume. If the freshly valued account is then below a risk threshold, the usual redemption or liquidation rules can apply.

## Dynamic Interest Rates

### Base Rates vs Dynamic Adjustments

**Important**: Your normal interest rate is the weighted average from your collateral mix (as explained in "Weighted Debt Terms" above). Dynamic rate adjustments are an emergency mechanism that only activates during severe market stress — this is NOT the default state.

### When Dynamic Rates Activate

Ripe monitors a configured GREEN reference pool as a health indicator. Under normal conditions, you simply pay your weighted base rate. Dynamic adjustments only activate when a duration-weighted history of confirmed pool observations places the GREEN ratio at or above the configured danger threshold.

**Key Point**: Dynamic rates are a protective mechanism that may never activate. They exist to incentivize market corrections during extreme conditions, not to penalize everyday borrowing.

### Building the Pool Signal

The protocol deliberately avoids treating one pool reading as sustained market stress:

1. **Chronological snapshots**: Confirmed pool observations are walked in time order.
2. **Corroborated intervals**: For each consecutive pair, the interval uses the lower of the two GREEN ratios. Both endpoints must therefore support a high reading before that interval can raise the signal.
3. **Duration weighting**: Each qualifying interval contributes in proportion to the number of protocol blocks between its endpoints. Longer corroborated conditions carry more weight than brief ones.
4. **Freshness boundaries**: Intervals with excessive gaps are excluded, the latest confirmed observation must remain fresh, and no unobserved "live tail" is extrapolated beyond it.

The result is a duration-weighted GREEN ratio, not a spot quote and not a simple average of snapshot values.

### Ratio Boost, Duration Boost, and Cap

Let:

* `b` be the user's weighted base borrowing rate
* `r` be the duration-weighted GREEN ratio
* `T` be the configured danger threshold
* `D` be the accumulated number of corroborated danger blocks
* `C` be the configured maximum borrowing rate

When `r < T`, the dynamic mechanism returns `b`. When `r ≥ T`, the excess ratio is normalized across the range from `T` to 100%. That value selects a configured ratio boost between its minimum and maximum bounds. A separate duration boost grows with `D`.

Conceptually:

```
ratioPosition = (r - T) / (100% - T)
ratioBoost = configured minimum + ratioPosition × configured boost range
dynamicRate = min(b + b × ratioBoost + durationBoost(D), C)
```

The ratio component is relative to the user's base rate. The duration component is additive. The final cap bounds their combined effect.

### Sustained Danger and Confirmed Recovery

Danger duration grows only across an interval whose two endpoints are both at or above `T`. A lone high observation creates no danger interval. If two corroborating high observations are `Δ` protocol blocks apart, that interval can add `Δ` danger blocks.

Recovery is also corroborated. Mixed high/low intervals, stale gaps, or unavailable observations neither add danger duration nor erase it. Consecutive safe endpoints accumulate recovery time, and the stored danger duration resets only after that safe history covers the configured recovery window.

While the duration history is retained, the dynamic adjustment still applies only when the current weighted ratio is in danger. If the weighted ratio becomes safe, the borrower returns to the base rate; if danger resumes before confirmed recovery completes, the retained duration history can contribute again.

### Underscore Earn Vault Integration

[Underscore Protocol's](https://underscore.finance/) Earn Vaults can use two different relationships with Ripe when the compatible registry, vault, and terms are configured. Core Vault shares can be listed as collateral, while a recognized Amplified Vault can receive special borrowing terms when it uses Ripe inside the strategy:

* **Configured discount** on borrowing rates
* **No origination fee** (Daowry waived)

**How It Works:**

Underscore Earn Vaults are automated yield strategies. Core Vaults power the base AI-managed yield layer and can be posted as collateral on Ripe. Amplified Vaults go one step further by borrowing GREEN from Ripe inside the strategy to add a second yield layer:

```
Illustrative standard borrowing rate: 6% APR
Illustrative recognized-vault rate: 3% APR (50% configured discount)
Origination fee: 0% (waived for Underscore)
```

**For Users:**

You don't interact with Ripe directly for this discount. Instead:

1. Deposit funds into an Underscore Earn Vault
2. Core Vaults run AI-managed yield strategies, while Amplified Vaults add Ripe borrowing inside the strategy
3. The vault — not you — receives the discounted rates when Ripe borrowing is used
4. The strategy receives the resulting economics and risks

**Why the Preferential Terms?**

The integration can pair preferential borrowing terms with an external strategy whose economics support the RIPE ecosystem. The recognized-vault check and configured discount are protocol controls; the external vault's performance and fee policy remain separate from Ripe's debt accounting.

## Borrowing Limits and Safety

### Multi-Tiered Limit System

Ripe implements several limits to ensure sustainable growth:

**1. Collateral-Based Limits**

* Fundamental constraint based on deposited value
* Maximum = Sum of (Asset Value × LTV Ratio)

**2. Per-User Debt Ceiling**

* Configured cap on one user's debt
* Governance can update the deployment's ceiling through its control path

**3. Global Debt Limit**

* Configured cap on aggregate borrower debt
* Bounds GREEN minting through the ordinary borrowing path
* Separate authorized mechanisms can have their own limits and accounting

**4. Interval Borrowing Limits**

* Time-based windows measured in the protocol's economic `block.number` clock
* Prevents flash loan attacks
* Smooths borrowing demand

**5. Minimum Debt Requirement**

* Configured by governance for the deployment
* Ensures position economic viability
* Reduces system complexity

When borrowing, the most restrictive limit applies. This creates a robust framework that protects both individual users and the protocol.

## The Borrowing Experience

### Step-by-Step Process

1. **Deposit Collateral**: Add assets to Ripe vaults
2. **Calculate Capacity**: System determines your borrowing power
3. **Choose Amount**: Borrow up to your available limit
4. **Pay Origination Fee**: A configured one-time fee applies when enabled
5. **Choose How to Receive**: Receive GREEN, convert it to sGREEN, or—when configured—convert it to sGREEN and deposit into Stability

### Distribution Options

When borrowing, you can choose one of three ways to receive your funds:

**Option 1: Direct GREEN**

* Receive standard GREEN stablecoins
* Use or transfer it subject to GREEN's token controls
* Most flexible option

**Option 2: Auto-Convert to** [**sGREEN**](../earning-and-rewards/01-sgreen.md)

* Borrowed GREEN automatically wrapped into yield-bearing [sGREEN](../earning-and-rewards/01-sgreen.md)
* Start earning yield immediately on borrowed funds
* Potential for positive carry (yield > borrow rate)
* No separate conversion transaction needed

**Option 3: Direct to** [**Stability Pool**](../earning-and-rewards/02-stability-pools.md)

* Borrowed GREEN converted to sGREEN and deposited into [Stability Pool](../earning-and-rewards/02-stability-pools.md) in one transaction
* Combine sGREEN economics, liquidation-claim exposure, and configured [RIPE rewards](../earning-and-rewards/03-ripe-rewards.md)
* Participate in compatible liquidations through the configured settlement spread
* Settlement liquidity can become claimable collateral, so vault NAV is not the same as immediately withdrawable sGREEN

### Origination Fee (Daowry)

A configured one-time fee on new borrows that:

* Joins accrued borrower interest in the Credit Engine's revenue calculation
* Is split between the governance buyback recipient and [sGREEN](../earning-and-rewards/01-sgreen.md) according to the configured buyback ratio
* Is disabled for recognized Underscore borrowing and whenever the fee configuration is off

Illustrative example at an assumed 0.25% fee: Borrow 10,000 GREEN → Pay 25 GREEN fee → Receive 9,975 GREEN

## Repayment Flexibility

Ripe Protocol does not impose a maturity date or prepayment penalty:

* **No prepayment penalties** - Partial or full repayment does not incur an early-payment fee
* **No fixed terms** - Keep your loan as long as needed
* **Partial payments allowed** - Reduce debt incrementally
* **Protocol controls still apply** - Teller and Ledger availability, account locks, token controls, and ordinary transaction checks can block an attempt

When a repayment succeeds, the credited amount reduces debt in that transaction.

## The Future of DeFi Borrowing

Forget the old way. No more portfolio fragmentation. No more wasted collateral. No more choosing between earning yield or accessing liquidity.

With Ripe, supported collateral can contribute according to its configured terms. Compatible yield-bearing positions can retain their external share or exchange-rate economics, while the dynamic-rate, redemption, deleverage, and liquidation mechanisms respond to configured risk conditions.

This is borrowing rebuilt from first principles. One position that actually understands what a portfolio is.

***

_Ready to experience unified borrowing? Your supported portfolio can work together._

_For technical implementation details, see the_ [_Credit Engine Technical Documentation_](https://ripe-finance.gitbook.io/ripe-developers/core-lending/creditengine)_._
