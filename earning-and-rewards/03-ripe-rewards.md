---
description: How configured RIPE rewards accrue and are claimed
---

# RIPE Rewards: Configured Rewards for Protocol Use

RIPE rewards use a rewards-specific accounting allowance, separate from bond and Reserve Engine allowances. Lootbox participant entitlement accrues from elapsed blocks when checkpointed; for borrowers, stakers, voters, and general depositors, RIPE is minted when a successful claim consumes that entitlement. The same allowance can also fund configured Stability collateral-claim rewards and a separately authorized, interval-gated Underscore distribution.

> **Examples, not live terms:** All assets, percentages, rates, prices, lock terms, tables, and scenario outputs on this page are illustrative. See [RIPE Params](https://params.ripe.finance) for current onchain configuration.

## Quick Start: Understanding Your Rewards

**🎯 The One-Minute Version:**

```
ELIGIBLE ACTIVITY      →  REWARD CATEGORY  →  YOUR SHARE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Governance/Stability   →  Stakers          →  Vault/asset staker-point share × user balance-point share
Borrowing GREEN        →  Borrowers        →  Principal × elapsed blocks
Eligible deposits      →  General          →  Vault/asset USD-point share × user balance-point share
Configured assets      →  Voters           →  Vault/asset voter-point share × user balance-point share

Category allocations and configured asset weights feed separate point totals; claims normalize against the relevant category and vault/asset totals.
```

**💡 Key Insight**: Participant activity in one category does not dilute another category's configured allocation—a borrower's points do not enter the staker calculation. Configuration weights still determine each category's share. Within a deposit-based category, both a vault/asset pool's share of global category points and the user's share of that pool's balance points affect the claim.

## The Reward Engine

### Continuous Token Flow

Unlike traditional yield farming with discrete epochs, Ripe's rewards flow continuously:

* **Every Block Counts**: Elapsed blocks add allowance-capped entitlement under the configuration used at checkpoint; they do not mint tokens every block
* **Participant Mint on Claim**: Ordinary Lootbox participant RIPE mints during a successful claim, subject to mint authority and circuit breakers
* **Configurable Distribution**: Category splits, asset allocations, point eligibility, and claim settings can change

A configured Underscore distribution is a separate path, not a participant claim. An authorized Switchboard action can invoke it only after its interval and allowance checks pass; the route checkpoints participant accrual, draws from the remaining reward allowance, and mints the allowance-capped deposit-reward and yield-bonus amounts to the configured Underscore distributor.

### The Points System

Ripe uses an elegant points mechanism that rewards both size and commitment:

```
Balance Points = Eligible Position Measure × Elapsed Blocks
```

These Lootbox reward balance points determine a user's share within a vault/asset position. They are separate from the governance points recorded by RipeGov and passed to a configured Boardroom integration.

## Configured Reward Categories

The protocol can distribute rewards across configured participant groups:

### 1. Stakers 💎

Eligible positions in configured staker vaults can participate in the staker category:

[**Governance Vault**](../governance-and-economics/02-governance.md) **(RIPE & RIPE LP)**

* **Reward Balance Points**: A lock-adjusted position measure × elapsed blocks determines the user's share within that governance vault/asset pool
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
* **USD-Weighted**: Eligible aggregate deposit value for each vault/asset pool determines that pool's category points

## Understanding Your Share

### Asset-Specific Weights

Each supported asset can have configuration values that accrue points within the relevant reward category:

Reward balance points and their user-share denominators are tracked separately for each vault/asset position. The same token held through two different vaults therefore participates through two distinct vault/asset pools, even though the token's configured weight remains asset-level.

* **Staker Points Weight**: Accrues staker asset points over elapsed blocks
  * Examples can include RIPE and RIPE LP in [Governance Vaults](../governance-and-economics/02-governance.md), and [sGREEN](01-sgreen.md) and GREEN LP in [Stability Pools](02-stability-pools.md), when configured
  * Hypothetical weights:
    * RIPE LP: 45
    * GREEN LP: 25
    * sGREEN: 15
    * RIPE: 15
* **Voter Points Weight**: Accrues voter asset points over elapsed blocks
  * Aggregate configured voter weights normalize the relevant asset-point shares
  * An asset can have both nonzero staker and voter weights, subject to the configured combined bound

**Important**: These values are normalized weights, not guaranteed literal percentages. If one vault/asset pool's accumulated voter points equal half of the global voter-point total, that pool's participants share half of the voter-category rewards available for that claim calculation.

### How Rewards Actually Flow (Simplified)

Think of RIPE rewards as category allocation followed by the applicable point ratios:

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
    │ Second Split │ (by vault/asset within category)
    └───────┬───────┘
            ↓
    Each eligible vault/asset pool accrues points
    using its asset's configured point weight
            ↓
    Your share based on
    your points vs that pool's total

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

**🎯 Step 3: What's YOUR Share of Your Vault/Asset Pool?**

```
Your Share = Your accumulated balance points ÷ Total accumulated balance points for that vault/asset pool

Example: You stake 1,000 RIPE for 100 blocks
- Your points: 1,000 × 100 = 100,000
- Total RIPE points in that governance vault/asset pool: 1,000,000
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

Assumptions for the example below: 500 RIPE daily entitlement at an assumed $10 per RIPE token, with the category and asset weights shown above and one eligible vault/asset pool per named asset. Position ratios mean a user's accumulated reward balance points divided by the total reward balance points for that vault/asset pool. Current balances alone match only when histories and weights align; for a governance-vault position, the lock-adjusted Lootbox position measure is already reflected in those reward balance points.

**For RIPE LP (45% of stakers in this example):**

```
Daily Rewards ≈ (Your RIPE LP Reward Balance Points / Total RIPE LP Reward Balance Points for That Vault/Asset Pool) × 202.5 RIPE

Example: a $100,000 RIPE LP position representing 1% of that vault/asset pool's reward balance points
= 1% × 202.5 = 2.025 RIPE per day
= $20.25 per day
= ~7.4% APR in USD terms
```

**For GREEN LP (25% of stakers):**

```
Daily Rewards ≈ (Your GREEN LP Reward Balance Points / Total GREEN LP Reward Balance Points for That Vault/Asset Pool) × 112.5 RIPE

Example: a $100,000 GREEN LP position representing 1% of that vault/asset pool's reward balance points
= 1% × 112.5 = 1.125 RIPE per day
= $11.25 per day
= ~4.1% APR in USD terms
```

**For RIPE Staking (15% of stakers):**

```
Daily Rewards ≈ (Your RIPE Reward Balance Points / Total RIPE Reward Balance Points for That Vault/Asset Pool) × 67.5 RIPE

Example: 10,000 RIPE staked with a 3-year lock, representing 3% of that vault/asset pool's reward balance points
= 3% × 67.5 = 2.025 RIPE per day
= $20.25 per day
= Your stake worth $100,000, earning $20.25/day = ~7.4% APR
With compounding: ~7.6% APY
```

**For sGREEN Deposits (15% of stakers):**

```
Daily Rewards ≈ (Your sGREEN Reward Balance Points / Total sGREEN Reward Balance Points for That Vault/Asset Pool) × 67.5 RIPE

Example: a $100,000 sGREEN position representing 2% of that vault/asset pool's reward balance points
= 2% × 67.5 = 1.35 RIPE per day
= $13.50 per day
= ~4.9% APR in USD terms
```

**For Borrowing GREEN (10% of the hypothetical entitlement):**

```
Daily Rewards ≈ (Your Borrow Points / Total Borrow Points) × 50 RIPE

Example: Your accumulated Borrow Points equal 5% of global Borrow Points at the claim calculation
= 5% × 50 = 2.5 RIPE per day
= $25.00 per day
= Reward APR cannot be inferred without a representative debt principal; borrowing costs remain separate
```

Current debt share does not necessarily equal Borrow Point share; principal history and elapsed blocks determine the accumulated ratio.

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

For staker, voter, and general-depositor claims, two nested ratios determine the user's share after the category allocation:

1. **Vault/asset share**: that pool's category points divided by global points for the category
2. **User share**: the user's balance points divided by total balance points for that vault/asset pool

The claim multiplies the category allocation by both ratios. Borrower claims use one ratio instead: the user's accumulated Borrow Points divided by global Borrow Points.

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

**Example**: Your accumulated RIPE reward balance points equal 1% of the total for that governance vault/asset pool → 67.5 × 1% = 0.675 RIPE per day = $6.75/day

### "What happens when I claim?"

The protocol applies configured auto-staking parameters:

**Example Settings (Protocol-Controlled):**

* **Auto-stake percentage**: Assume a configured 75% ratio for this example
* **Lock duration**: Assume approximately 1 year for this example
* **Your choice**: Claim now or wait, and optionally request that the entire claim be staked

**The Full Claim Process:**

When you claim RIPE rewards, the Lootbox contract:
1. Iterates through your registered vaults and assets, settling each eligible vault/asset claim and summing the resulting RIPE entitlement
2. Determines your share of each reward category (stakers, borrowers, etc.)
3. Calculates allowance-capped RIPE entitlement from your points versus the relevant totals
4. Mints RIPE only as the claim succeeds and applies the configured liquid/auto-staked split
5. Deposits the auto-staked portion into the current core governance vault resolved through Mission Control

**Example Claim:**

* You have 1,000 RIPE rewards to claim
* Assume the configured claim terms require 75% auto-stake with an approximately 1-year lock
* Result: 250 RIPE to your wallet + 750 RIPE locked in governance vault

**Delegation**: Others can claim on your behalf if you've granted `canClaimLoot` permission. This enables automated compounding strategies.

**Why This Matters**: The auto-staked portion becomes a governance-vault position subject to its configured lock and withdrawal controls. It can accrue points when enabled, but neither token value nor governance authority is guaranteed.

### "How do same-token positions compete?"

Users in the same vault/asset pool compete against that pool's total balance points. The same token held through another vault has a separate balance-point denominator. Each vault/asset pool's category points then compete with other eligible pools against the global points for that reward category.

### "What happens if nobody stakes or borrows?"

A configured category is not automatically redirected to another category just because it has no active participants. Borrower claims require nonzero user and global borrow points plus available borrower rewards. Deposit claims use user balance points, vault/asset balance points, the relevant vault/asset and global category points, and available rewards for that category.

## Returns Are Not Guaranteed

Point totals, category balances, asset weights, emission settings, token prices, and participant activity can all change. The examples above explain the accounting; they are not promised rates, returns, or live configuration.

***

_For technical implementation details, see_ [_Lootbox Technical Documentation_](https://ripe-finance.gitbook.io/ripe-developers/treasury-and-rewards/lootbox)_._
