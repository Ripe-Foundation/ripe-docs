---
description: How configured RIPE rewards accrue and are claimed
---

# RIPE Rewards: Configured Rewards for Protocol Use

RIPE rewards use a rewards-specific accounting allowance, separate from bond and Reserve Engine allowances. Entitlement accrues from elapsed blocks when checkpointed; RIPE is minted only when a claim succeeds.

> **Examples, not live terms:** All assets, percentages, rates, prices, lock terms, tables, and scenario outputs on this page are illustrative. See [RIPE Params](https://params.ripe.finance) for current onchain configuration.

## Quick Start: Understanding Your Rewards

**🎯 The One-Minute Version:**

```
ELIGIBLE ACTIVITY      →  REWARD CATEGORY  →  YOUR SHARE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Governance/Stability   →  Stakers          →  Asset staker points × user balance share
Borrowing GREEN        →  Borrowers        →  Principal × elapsed blocks
Eligible deposits      →  General          →  Asset USD points × user balance share
Configured assets      →  Voters           →  Asset voter points × user balance share

Category and asset weights normalize against their relevant configured totals.
```

**💡 Key Insight**: You don't compete with all users — only those in your specific pool. A borrower doesn't dilute a staker's rewards!

## The Reward Engine

### Continuous Token Flow

Unlike traditional yield farming with discrete epochs, Ripe's rewards flow continuously:

* **Every Block Counts**: Elapsed blocks add allowance-capped entitlement under the configuration used at checkpoint; they do not mint tokens every block
* **Mint on Claim**: RIPE mints only during a successful claim, subject to mint authority and circuit breakers
* **Configurable Distribution**: Category splits, asset allocations, point eligibility, and claim settings can change

### The Points System

Ripe uses an elegant points mechanism that rewards both size and commitment:

```
Balance Points = Eligible Position Measure × Elapsed Blocks
```

These Lootbox reward balance points determine a user's share within an asset position. They are separate from the governance points recorded by RipeGov and passed to a configured Boardroom integration.

## Configured Reward Categories

The protocol can distribute rewards across configured participant groups:

### 1. Stakers 💎

Eligible positions in configured staker vaults can participate in the staker category:

[**Governance Vault**](../governance-and-economics/02-governance.md) **(RIPE & RIPE LP)**

* **Reward Balance Points**: A lock-adjusted position measure × elapsed blocks determines the user's share within that governance asset
* **Asset Staker Weight**: Each eligible asset can have a different configured weight that accrues its share of the staker category
* **Claim-Time Staking**: A configured share of a successful RIPE claim can be deposited into governance

[**Stability Pools**](02-stability-pools.md) **(**[**sGREEN**](01-sgreen.md) **& GREEN LP)**

* **Separate Outcomes**: Configured RIPE entitlement is distinct from the deposited asset's economics and liquidation-settlement result
* **Vault Controls**: Deposit and withdrawal availability follows the configured Stability vault and asset controls
* **Settlement Role**: Deposited liquidity can fund compatible liquidation settlement; outcomes remain conditional

### 2. Borrowers 💰

GREEN debt can accrue borrower points when the borrower category is configured:

* **Reward Basis**: Outstanding GREEN principal × time borrowed
* **Relative Share**: Larger principal held for more blocks contributes more borrower points
* **Separate Economics**: Any RIPE entitlement is separate from interest, liquidation risk, and other borrowing costs

## Additional Reward Categories

### Vote Depositors

Assets with configured voter weights accrue voter points independently from the staker category:

* **Configured Selection**: Voter weights determine which assets accrue points in this category
* **Independent Weighting**: Voter points can coexist with staker points for the same asset

### General Depositors

Can reward eligible vault deposits when the required configuration is present:

* **Top-Level Allocation Required**: The category must receive a configured share
* **Asset-Level Gate**: An asset enters this branch only when its staker-points allocation is zero
* **USD-Weighted**: Eligible aggregate deposit value determines the asset's points

## Understanding Your Share

### Asset-Specific Weights

Each supported asset can have configuration values that accrue points within the relevant reward category:

* **Staker Points Weight**: Accrues staker asset points over elapsed blocks
  * Only applies to staked assets (RIPE, RIPE LP in [Governance Vault](../governance-and-economics/02-governance.md); [sGREEN](01-sgreen.md), GREEN LP in [Stability Pools](02-stability-pools.md))
  * Hypothetical weights:
    * RIPE LP: 45
    * GREEN LP: 25
    * sGREEN: 15
    * RIPE: 15
* **Voter Points Weight**: Accrues voter asset points over elapsed blocks
  * Aggregate configured voter weights normalize the relevant asset-point shares
  * An asset can have both nonzero staker and voter weights, subject to the configured combined bound

**Important**: These values are normalized weights, not guaranteed literal percentages. If the top-level voter weight is 20 and Asset A contributes half of the accumulated voter asset points, Asset A participants share half of the voter-category rewards available for that claim calculation.

### How Rewards Actually Flow (Simplified)

Think of RIPE rewards like a waterfall with two splits:

```
Hypothetical RIPE Entitlement (500 RIPE/day equivalent)
            ↓
    ┌───────┴───────┐
    │ First Split  │ (by user type)
    └───────┬───────┘
            ↓
    Assumed weights: 90 → Stakers
                     10 → Borrowers

    Assumed weights: 0 → Voters
                     0 → General Depositors
            ↓
    ┌───────┴───────┐
    │ Second Split │ (by asset within pool)
    └───────┬───────┘
            ↓
    Each asset accrues its
    configured point weight
            ↓
    Your share based on
    your points vs total

```

### Step-by-Step Calculation Guide

Let's follow your RIPE rewards step by step:

**🎯 Step 1: Where Do Your Rewards Come From?**

```
If you're staking RIPE → You can accrue from the Stakers category
If you're borrowing → You can accrue from the Borrowers category
General deposits and voter-weighted assets can accrue when configured
```

**🎯 Step 2: What's Your Asset's Share?**

```
Hypothetical daily entitlement: 500 RIPE at an assumed $10 per token = $5,000/day
Stakers pool gets 90%: 450 RIPE/day
Borrowers pool gets 10%: 50 RIPE/day

Within the Stakers pool (450 RIPE/day):
- RIPE LP tokens get 45% = 202.5 RIPE/day
- GREEN LP tokens get 25% = 112.5 RIPE/day
- sGREEN gets 15% = 67.5 RIPE/day
- RIPE tokens get 15% = 67.5 RIPE/day
```

**🎯 Step 3: What's YOUR Share of Your Asset Pool?**

```
Your Share = Your accumulated balance points ÷ Total accumulated balance points for that asset

Example: You stake 1,000 RIPE for 100 blocks
- Your points: 1,000 × 100 = 100,000
- Total RIPE points: 1,000,000
- Your share: 100,000 ÷ 1,000,000 = 10%
```

**🎯 Step 4: Calculate Your Rewards**

```
Your rewards = Your share × Asset's daily allocation

Example: 10% share of RIPE staking pool
10% × 67.5 RIPE/day = 6.75 RIPE per day
At $10 per RIPE = $67.50 per day earned!
```

### Hypothetical Quick Reference Table

| Your Action              | Pool You Earn From        | How to Maximize                      |
| ------------------------ | ------------------------- | ------------------------------------ |
| Provide RIPE LP          | Stakers (90%) - 45% share | Larger positions earn more           |
| Provide GREEN LP         | Stakers (90%) - 25% share | Larger positions earn more           |
| Stake RIPE               | Stakers (90%) - 15% share | Lock for 3 years (+200% bonus)       |
| Deposit sGREEN           | Stakers (90%) - 15% share | Combine with stability pool benefits |
| Borrow GREEN             | Borrowers (10%)           | Larger, longer loans                 |
| General deposits         | General (when configured) | Eligible USD value and elapsed blocks |
| Voter-weighted assets    | Voters (when configured)  | Asset and user point share            |

### Hypothetical Rewards Estimator

**"How much will I earn?"** - Quick formulas for common scenarios:

Assumptions for the example below: 500 RIPE daily entitlement at an assumed $10 per RIPE token, with the category and asset weights shown above. Position ratios mean a user's accumulated reward balance points divided by that asset's total reward balance points. Current balances alone match only when histories and weights align; for a governance-vault position, the lock-adjusted Lootbox position measure is already reflected in those reward balance points.

**For RIPE LP (45% of stakers in this example):**

```
Daily Rewards ≈ (Your RIPE LP Reward Balance Points / Total RIPE LP Reward Balance Points) × 202.5 RIPE

Example: a $100,000 RIPE LP position representing 1% of that asset's reward balance points
= 1% × 202.5 = 2.025 RIPE per day
= $20.25 per day
= ~7.4% APR in USD terms
```

**For GREEN LP (25% of stakers):**

```
Daily Rewards ≈ (Your GREEN LP Reward Balance Points / Total GREEN LP Reward Balance Points) × 112.5 RIPE

Example: a $100,000 GREEN LP position representing 1% of that asset's reward balance points
= 1% × 112.5 = 1.125 RIPE per day
= $11.25 per day
= ~4.1% APR in USD terms
```

**For RIPE Staking (15% of stakers):**

```
Daily Rewards ≈ (Your RIPE Reward Balance Points / Total RIPE Reward Balance Points) × 67.5 RIPE

Example: 10,000 RIPE staked with a 3-year lock, representing 3% of that asset's reward balance points
= 3% × 67.5 = 2.025 RIPE per day
= $20.25 per day
= Your stake worth $100,000, earning $20.25/day = ~7.4% APR
With compounding: ~7.6% APY
```

**For sGREEN Deposits (15% of stakers):**

```
Daily Rewards ≈ (Your sGREEN Reward Balance Points / Total sGREEN Reward Balance Points) × 67.5 RIPE

Example: a $100,000 sGREEN position representing 2% of that asset's reward balance points
= 2% × 67.5 = 1.35 RIPE per day
= $13.50 per day
= ~4.9% APR in USD terms
```

**For Borrowing GREEN (10% of the hypothetical entitlement):**

```
Daily Rewards ≈ (Your Borrow Points / Total Borrow Points) × 50 RIPE

Example: $500,000 borrowed (5% of total debt)
= 5% × 50 = 2.5 RIPE per day
= $25.00 per day
= ~1.8% APR in hypothetical rewards before borrowing costs
```

## Auto-Staking Mechanism

### How Auto-Staking Works

Reward claims apply the configured auto-staking settings:

* **Stake Ratio**: The configured portion is auto-staked and the remainder is liquid. A zero ratio permits a fully liquid normal claim; the claimant may still choose to stake the entire claim
* **Lock Duration**: Derived from configured duration settings and the current core governance-vault lock bounds

**Why This Matters**: Auto-staking routes the configured portion of a successful claim into the governance vault instead of the claimant's liquid balance. That portion follows the vault's lock and withdrawal controls and can accrue governance points when enabled; those points do not by themselves grant contract authority.

## Protocol Configuration

### Flexible Parameters

Mission Control stores the reward configuration used by Lootbox:

* **Emission Rate**: RIPE entitlement accounted per elapsed block at checkpoint
* **Category Weights**: Normalized allocation among participant types
* **Asset Weights**: Values that accrue asset points relative to the applicable configured totals
* **Auto-Stake Settings**: Default ratios and durations

### Configuration Authority

The contracts and governance roles configured for a deployment determine who can update:

* Emission schedules and rates
* Category allocation adjustments
* Asset-specific incentives
* New reward mechanisms

## Common Questions About Rewards

### "Why do I calculate my share twice?"

You don't! Think of it as one calculation with two inputs:

1. **Which pie you're eating from** (Staker, Borrower, etc.)
2. **How big your slice is** (Your percentage of that pie)

It's like a buffet where desserts are on one table and mains on another — you only compete with people at your table, not the whole restaurant!

### "How do I estimate my rewards?"

**Simple Method:**

Using the hypothetical configuration above:

1. Find your pool's daily RIPE allocation:
   * Stakers total: 450 RIPE/day (90% of 500 RIPE)
   * Borrowers total: 50 RIPE/day (10% of 500 RIPE)
2. For stakers, find your asset's share:
   * RIPE LP: 202.5 RIPE/day (45% of stakers)
   * GREEN LP: 112.5 RIPE/day (25% of stakers)
   * sGREEN: 67.5 RIPE/day (15% of stakers)
   * RIPE: 67.5 RIPE/day (15% of stakers)
3. Multiply: asset allocation × your percentage = daily rewards

**Example**: You have 1% of all staked RIPE → 67.5 × 1% = 0.675 RIPE per day = $6.75/day

### "What happens when I claim?"

The protocol applies configured auto-staking parameters:

**Example Settings (Protocol-Controlled):**

* **Auto-stake percentage**: Assume a configured 75% ratio for this example
* **Lock duration**: Assume approximately 1 year for this example
* **Your choice**: Claim now or wait, and optionally request that the entire claim be staked

**The Full Claim Process:**

When you claim RIPE rewards, the Lootbox contract:
1. Iterates through ALL your vaults and assets to calculate total points
2. Determines your share of each reward pool (stakers, borrowers, etc.)
3. Calculates allowance-capped RIPE entitlement from your points versus the relevant totals
4. Mints RIPE only as the claim succeeds and applies the configured liquid/auto-staked split
5. Deposits the auto-staked portion into the current core governance vault resolved through Mission Control

**Example Claim:**

* You have 1,000 RIPE rewards to claim
* Assume the configured claim terms require 75% auto-stake with an approximately 1-year lock
* Result: 250 RIPE to your wallet + 750 RIPE locked in governance vault

**Delegation**: Others can claim on your behalf if you've granted `canClaimLoot` permission. This enables automated compounding strategies.

**Why This Matters**: The auto-staked portion becomes a governance-vault position subject to its configured lock and withdrawal controls. It can accrue points when enabled, but neither token value nor governance authority is guaranteed.

### "Do different assets in the same pool compete?"

Yes, within each pool! For example, in the Stakers pool:

* RIPE stakers compete with other RIPE stakers
* RIPE LP stakers compete with other RIPE LP stakers
* Their assets share the Stakers category according to accumulated asset points relative to the global staker-point total

### "What happens if nobody stakes or borrows?"

A configured category is not automatically redirected to another category just because it has no active participants. Borrower claims require nonzero user and global borrow points plus available borrower rewards. Deposit claims use user balance points, asset balance points, the relevant asset and global category points, and available rewards for that category.

## Returns Are Not Guaranteed

Point totals, category balances, asset weights, emission settings, token prices, and participant activity can all change. The examples above explain the accounting; they are not promised rates, returns, or live configuration.

***

_For technical implementation details, see_ [_Lootbox Technical Documentation_](https://ripe-finance.gitbook.io/ripe-developers/treasury-and-rewards/lootbox)_._
