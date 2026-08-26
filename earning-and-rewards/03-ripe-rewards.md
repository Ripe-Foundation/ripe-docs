---
description: Get Paid to Use the Protocol
---

# RIPE Rewards: Get Paid to Use the Protocol

Every block. Every transaction. Every dollar borrowed or staked.

Ripe Protocol is watching. And paying.

RIPE rewards use a rewards-specific accounting allowance, separate from bond and Reserve Engine allowances. Entitlement accrues from elapsed blocks when checkpointed; RIPE is minted only when a claim succeeds.

The best part? Early users are sharing a tiny pool. More rewards per person until the masses arrive.

> **Examples, not live terms:** All assets, percentages, rates, prices, lock terms, tables, and scenario outputs on this page are illustrative. See [RIPE Params](https://params.ripe.finance) for current onchain configuration.

## Quick Start: Understanding Your Rewards

**🎯 The One-Minute Version:**

```
YOUR ACTIVITY          →  REWARD POOL  →  YOUR SHARE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Staking (All Types)    →  90% of RIPE  →  Based on asset type + position
  • RIPE/RIPE LP      →  Governance   →  Lock bonus up to 3x
  • sGREEN/GREEN LP   →  Stability    →  Size × time × weight
Borrowing GREEN        →  10% of RIPE  →  Based on debt size

OPTIONAL ALLOCATIONS (when configured):
Depositing any asset   →  TBD % of RIPE  →  Based on USD value
Future voted assets    →  TBD % of RIPE  →  Community decides

SIMPLE FORMULA: Your % of pool × Pool rewards = Your RIPE
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

This simple formula creates profound fairness — a smaller position held longer can earn more than a whale's brief deposit. It's democracy through mathematics.

## Current Reward Categories

The protocol can distribute rewards across configured participant groups:

### 1. Stakers (90% of Emissions) 💎

Staking in protocol vaults earns the lion's share of rewards:

[**Governance Vault**](../governance-and-economics/02-governance.md) **(RIPE & RIPE LP)**

* **Base Rewards**: Size × time × asset weight
* **Lock Multiplier**: Up to 3x boost for maximum duration locks
* **LP Advantage**: RIPE LP tokens earn 50% more points than RIPE
* **Compound Strategy**: Auto-stake rewards for exponential growth

[**Stability Pools**](02-stability-pools.md) **(**[**sGREEN**](01-sgreen.md) **& GREEN LP)**

* **Dual Yield**: RIPE rewards plus liquidation profits
* **No Lock Required**: Flexible liquidity with full rewards
* **Risk Buffer**: Help secure the protocol while earning

### 2. Borrowers (10% of Emissions) 💰

Taking out GREEN loans earns rewards proportional to your debt:

* **Reward Basis**: Outstanding GREEN principal × time borrowed
* **Why It Matters**: Borrowing creates GREEN demand and protocol revenue
* **Smart Strategy**: Larger, longer-term loans maximize rewards
* **Real Benefit**: Offset borrowing costs with RIPE earnings

## Additional Reward Categories

### Vote Depositors

When governance activates, token holders may vote to allocate rewards to specific assets:

* **Democratic Selection**: Community chooses reward-earning assets
* **Targeted Incentives**: Direct liquidity where protocol needs it most
* **Strategic Deposits**: Align your holdings with governance decisions

### General Depositors

Can reward eligible vault deposits when the required configuration is present:

* **Top-Level Allocation Required**: The category must receive a configured share
* **Asset-Level Gate**: An asset enters this branch only when its staker-points allocation is zero
* **USD-Weighted**: Eligible aggregate deposit value determines the asset's points

## Understanding Your Share

### Asset-Specific Allocations

Each supported asset has its own configuration that determines how it splits the top-level reward pools:

* **Staker Points Allocation**: Percentage of the total Stakers pool this asset receives
  * Only applies to staked assets (RIPE, RIPE LP in [Governance Vault](../governance-and-economics/02-governance.md); [sGREEN](01-sgreen.md), GREEN LP in [Stability Pools](02-stability-pools.md))
  * Example allocations:
    * RIPE LP: 45% (highest rewards!)
    * GREEN LP: 25%
    * sGREEN: 15%
    * RIPE: 15%
* **Voter Points Allocation**: Percentage of the Vote Depositors pool this asset receives
  * Only for assets selected through governance voting
  * Aggregate configured voter allocations normalize the relevant asset-point shares
  * Staked assets have 0% voter allocation since they earn from the Stakers pool

**Important**: These percentages split their respective category pools. If Vote Depositors receive 20% of total emissions and Asset A has 50% voter allocation, Asset A depositors share 10% of total emissions (50% of 20%).

### How Rewards Actually Flow (Simplified)

Think of RIPE rewards like a waterfall with two splits:

```
Total RIPE Emissions (500 RIPE/day)
            ↓
    ┌───────┴───────┐
    │ First Split  │ (by user type)
    └───────┬───────┘
            ↓
    90% → Stakers Pool
    10% → Borrowers Pool

    Future: Vote & General Pools
    (Currently 0%, may change)
            ↓
    ┌───────┴───────┐
    │ Second Split │ (by asset within pool)
    └───────┬───────┘
            ↓
    Each asset gets its
    configured percentage
            ↓
    Your share based on
    your points vs total

```

### Step-by-Step Calculation Guide

Let's follow your RIPE rewards step by step:

**🎯 Step 1: Where Do Your Rewards Come From?**

```
If you're staking RIPE → You earn from the Stakers pool (90%)
If you're borrowing → You earn from the Borrowers pool (10%)
Optional: General deposits and voted assets when configured
```

**🎯 Step 2: What's Your Asset's Share?**

```
Daily emissions: 500 RIPE at $10 per token = $5,000/day
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

### Quick Reference Table

| Your Action              | Pool You Earn From        | How to Maximize                      |
| ------------------------ | ------------------------- | ------------------------------------ |
| Provide RIPE LP          | Stakers (90%) - 45% share | Larger positions earn more           |
| Provide GREEN LP         | Stakers (90%) - 25% share | Larger positions earn more           |
| Stake RIPE               | Stakers (90%) - 15% share | Lock for 3 years (+200% bonus)       |
| Deposit sGREEN           | Stakers (90%) - 15% share | Combine with stability pool benefits |
| Borrow GREEN             | Borrowers (10%)           | Larger, longer loans                 |
| Future: General deposits | Not active yet            | TBD                                  |
| Future: Voted assets     | Not active yet            | TBD                                  |

### Simple Rewards Estimator

**"How much will I earn?"** - Quick formulas for common scenarios:

Assumptions: 500 RIPE daily emissions at $10 per RIPE token. Position ratios below mean accumulated balance points divided by total asset points; current balances alone match only when histories and weights align, and a governance lock is already reflected in those points.

**For RIPE LP (Highest Rewards - 45% of stakers):**

```
Daily Rewards ≈ (Your RIPE LP Points / Total RIPE LP Points) × 202.5 RIPE

Example: $100,000 in RIPE LP (1% of total)
= 1% × 202.5 = 2.025 RIPE per day
= $20.25 per day
= ~7.4% APR in USD terms
```

**For GREEN LP (25% of stakers):**

```
Daily Rewards ≈ (Your GREEN LP Points / Total GREEN LP Points) × 112.5 RIPE

Example: $100,000 in GREEN LP (1% of total)
= 1% × 112.5 = 1.125 RIPE per day
= $11.25 per day
= ~4.1% APR in USD terms
```

**For RIPE Staking (15% of stakers):**

```
Daily Rewards ≈ (Your RIPE Points / Total RIPE Points) × 67.5 RIPE

Example: 10,000 RIPE staked with a 3-year lock, representing 3% of accumulated points
= 3% × 67.5 = 2.025 RIPE per day
= $20.25 per day
= Your stake worth $100,000, earning $20.25/day = ~7.4% APR
With compounding: ~7.6% APY
```

**For sGREEN Deposits (15% of stakers):**

```
Daily Rewards ≈ (Your sGREEN Points / Total sGREEN Points) × 67.5 RIPE

Example: $100,000 in sGREEN (2% of total)
= 2% × 67.5 = 1.35 RIPE per day
= $13.50 per day
= ~4.9% APR in USD terms
```

**For Borrowing GREEN (10% of emissions):**

```
Daily Rewards ≈ (Your Borrow Points / Total Borrow Points) × 50 RIPE

Example: $500,000 borrowed (5% of total debt)
= 5% × 50 = 2.5 RIPE per day
= $25.00 per day
= ~1.8% APR in rewards (offsetting borrowing costs!)
```

## Auto-Staking Mechanism

### How Auto-Staking Works

The protocol enforces auto-staking to balance token distribution with long-term alignment:

* **Stake Ratio**: 75% must be auto-staked
  * Only 25% goes to your wallet as liquid RIPE
  * Prevents market flooding while building committed participants
* **Duration Ratio**: 33% × 3-year max = ~1-year lock
  * All auto-staked rewards are locked for approximately 1 year
  * Ensures reward recipients become long-term stakeholders

**Why This Matters**: Rather than dumping tokens on the market, auto-staking creates a community of invested participants who earn governance power alongside their rewards. You're not just earning tokens — you're earning a voice in the protocol's future.

## Protocol Configuration

### Flexible Parameters

Mission Control governs all reward settings:

* **Emission Rate**: RIPE entitlement accounted per elapsed block at checkpoint
* **Category Splits**: Percentage to each participant type
* **Asset Allocations**: Individual asset point multipliers
* **Auto-Stake Settings**: Default ratios and durations

### Governance Evolution

Once activated, RIPE holders will control:

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

The protocol enforces auto-staking parameters to align incentives:

**Example Settings (Protocol-Controlled):**

* **Auto-stake percentage**: 75% must be auto-staked
* **Lock duration**: ~1 year (33% of max 3-year duration)
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
* Protocol requires 75% auto-stake with ~1-year lock
* Result: 250 RIPE to your wallet + 750 RIPE locked in governance vault

**Delegation**: Others can claim on your behalf if you've granted `canClaimLoot` permission. This enables automated compounding strategies.

**Why This Matters**: Auto-staking prevents market flooding and ensures reward recipients become long-term stakeholders with governance power. You're earning both tokens AND future influence!

### "Do different assets in the same pool compete?"

Yes, within each pool! For example, in the Stakers pool:

* RIPE stakers compete with other RIPE stakers
* RIPE LP stakers compete with other RIPE LP stakers
* But they share the total Stakers allocation based on configured percentages

### "What happens if nobody stakes/borrows?"

More rewards for those who do! If you're the only RIPE staker, you get 100% of RIPE's allocation in the Stakers pool. Early participants often see highest returns.

## The Early Bird Gets the RIPE

Participant point totals and relative reward shares change over time.

Those juicy GREEN LP yields? They'll shrink when billions pour in. That insane RIPE staking APY? Only while the participant pool stays small.

Every block you wait is rewards you're not earning. Every day you delay is yield going to someone else.

The protocol pays those who show up. Are you showing up?

***

_For technical implementation details, see_ [_Lootbox Technical Documentation_](https://ripe-finance.gitbook.io/ripe-developers/treasury-and-rewards/lootbox)_._
