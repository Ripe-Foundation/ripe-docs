---
description: Get Paid to Use the Protocol
---

# RIPE Rewards: Get Paid to Use the Protocol

Every block. Every transaction. Every dollar borrowed or staked.

Ripe Protocol is watching. And paying.

250 million RIPE tokens are flowing to users who actually use the protocol (for block rewards and bonding). Not VCs. Not insiders. Users. Borrow GREEN? Get paid. Stake in pools? Get paid more. Lock your rewards? Get paid even more.

The best part? Early users are sharing a tiny pool. More rewards per person until the masses arrive.

## Quick Start: Understanding Your Rewards

**🎯 The One-Minute Version:**

```
YOUR ACTIVITY          →  REWARD POOL  →  YOUR SHARE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Staking (All Types)    →  90% of RIPE  →  Based on asset type + position
  • RIPE/RIPE LP      →  Governance   →  Lock bonus up to 3x
  • sGREEN/GREEN LP   →  Stability    →  Size × time × weight
Borrowing GREEN        →  10% of RIPE  →  Based on debt size

FUTURE ALLOCATIONS (Not Active Yet):
Depositing any asset   →  TBD % of RIPE  →  Based on USD value
Future voted assets    →  TBD % of RIPE  →  Community decides

SIMPLE FORMULA: Your % of pool × Pool rewards = Your RIPE
```

**💡 Key Insight**: You don't compete with all users — only those in your specific pool. A borrower doesn't dilute a staker's rewards!

## The Reward Engine

### Continuous Token Flow

Unlike traditional yield farming with discrete epochs, Ripe's rewards flow continuously:

* **Every Block Counts**: New RIPE tokens mint with each blockchain block
* **Real-Time Accumulation**: Your rewards grow second by second, not weekly or monthly
* **No Waiting Periods**: Start earning immediately upon participation
* **Fair Distribution**: Time-weighted system prevents gaming or manipulation

### The Points System

Ripe uses an elegant points mechanism that rewards both size and commitment:

```
Points = Position Size × Time Held (in blocks)
```

This simple formula creates profound fairness — a smaller position held longer can earn more than a whale's brief deposit. It's democracy through mathematics.

## Current Reward Categories

The protocol currently distributes rewards to two participant groups:

### 1. Stakers (90% of Emissions) 💎

Staking in protocol vaults earns the lion's share of rewards:

[**Governance Vault**](../governance-and-economics/09-governance.md) **(RIPE & RIPE LP)**

* **Base Rewards**: Size × time × asset weight
* **Lock Multiplier**: Up to 3x boost for maximum duration locks
* **LP Advantage**: RIPE LP tokens earn 50% more points than RIPE
* **Compound Strategy**: Auto-stake rewards for exponential growth

[**Stability Pools**](06-stability-pools.md) **(**[**sGREEN**](05-sgreen.md) **& GREEN LP)**

* **Dual Yield**: RIPE rewards plus liquidation profits
* **No Lock Required**: Flexible liquidity with full rewards
* **Risk Buffer**: Help secure the protocol while earning

### 2. Borrowers (10% of Emissions) 💰

Taking out GREEN loans earns rewards proportional to your debt:

* **Reward Basis**: Outstanding GREEN principal × time borrowed
* **Why It Matters**: Borrowing creates GREEN demand and protocol revenue
* **Smart Strategy**: Larger, longer-term loans maximize rewards
* **Real Benefit**: Offset borrowing costs with RIPE earnings

## Future Reward Categories (Not Active Yet)

### Vote Depositors (Future)

When governance activates, token holders may vote to allocate rewards to specific assets:

* **Democratic Selection**: Community chooses reward-earning assets
* **Targeted Incentives**: Direct liquidity where protocol needs it most
* **Strategic Deposits**: Align your holdings with governance decisions

### General Depositors (Future)

May be activated to reward all vault deposits:

* **USD-Weighted**: Fair distribution based on deposit value
* **Asset Agnostic**: All supported assets participate equally
* **Passive Income**: Earn just by holding assets in Ripe vaults

## Understanding Your Share

### Asset-Specific Allocations

Each supported asset has its own configuration that determines how it splits the top-level reward pools:

* **Staker Points Allocation**: Percentage of the total Stakers pool this asset receives
  * Only applies to staked assets (RIPE, RIPE LP in [Governance Vault](../governance-and-economics/09-governance.md); [sGREEN](05-sgreen.md), GREEN LP in [Stability Pools](06-stability-pools.md))
  * Current allocations:
    * GREEN LP: 65% (highest rewards!)
    * RIPE LP: 15%
    * sGREEN: 10%
    * RIPE: 10%
* **Voter Points Allocation**: Percentage of the Vote Depositors pool this asset receives
  * Only for assets selected through governance voting
  * All voter allocations across eligible assets must sum to 100%
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
Future: General deposits and voted assets (not active yet)
```

**🎯 Step 2: What's Your Asset's Share?**

```
Daily emissions: 500 RIPE at $10 per token = $5,000/day
Stakers pool gets 90%: 450 RIPE/day
Borrowers pool gets 10%: 50 RIPE/day

Within the Stakers pool (450 RIPE/day):
- GREEN LP tokens get 65% = 292.5 RIPE/day
- RIPE LP tokens get 15% = 67.5 RIPE/day
- sGREEN gets 10% = 45 RIPE/day
- RIPE tokens get 10% = 45 RIPE/day
```

**🎯 Step 3: What's YOUR Share of Your Asset Pool?**

```
Your Points = Amount × Time
Your Share = Your Points ÷ Total Points for that asset

Example: You stake 1,000 RIPE for 100 blocks
- Your points: 1,000 × 100 = 100,000
- Total RIPE points: 1,000,000
- Your share: 100,000 ÷ 1,000,000 = 10%
```

**🎯 Step 4: Calculate Your Rewards**

```
Your rewards = Your share × Asset's daily allocation

Example: 10% share of RIPE staking pool
10% × 45 RIPE/day = 4.5 RIPE per day
At $10 per RIPE = $45 per day earned!
```

### Quick Reference Table

| Your Action              | Pool You Earn From        | How to Maximize                      |
| ------------------------ | ------------------------- | ------------------------------------ |
| Provide GREEN LP         | Stakers (90%) - 65% share | Larger positions earn more           |
| Provide RIPE LP          | Stakers (90%) - 15% share | Larger positions earn more           |
| Stake RIPE               | Stakers (90%) - 10% share | Lock for 3 years (+200% bonus)       |
| Deposit sGREEN           | Stakers (90%) - 10% share | Combine with stability pool benefits |
| Borrow GREEN             | Borrowers (10%)           | Larger, longer loans                 |
| Future: General deposits | Not active yet            | TBD                                  |
| Future: Voted assets     | Not active yet            | TBD                                  |

### Simple Rewards Estimator

**"How much will I earn?"** - Quick formulas for common scenarios:

Assumptions: 500 RIPE daily emissions at $10 per RIPE token

**For GREEN LP (Highest Rewards - 65% of stakers):**

```
Daily Rewards ≈ (Your LP Value / Total GREEN LP) × 292.5 RIPE

Example: $100,000 in GREEN LP (1% of total)
= 1% × 292.5 = 2.925 RIPE per day
= $29.25 per day
= ~10.7% APR in USD terms
```

**For RIPE Staking (10% of stakers):**

```
Daily Rewards ≈ (Your RIPE / Total RIPE Staked) × 45 RIPE × Lock Multiplier

Example: 10,000 RIPE staked (1% of total) with 3-year lock
= 1% × 45 × 3.0 = 1.35 RIPE per day
= $13.50 per day
= Your stake worth $100,000, earning $13.50/day = ~4.9% APR
With compounding: ~5.0% APY
```

**For sGREEN Deposits (10% of stakers):**

```
Daily Rewards ≈ (Your sGREEN Value / Total sGREEN) × 45 RIPE

Example: $100,000 in sGREEN (2% of total)
= 2% × 45 = 0.9 RIPE per day
= $9.00 per day
= ~3.3% APR in USD terms
```

**For Borrowing GREEN (10% of emissions):**

```
Daily Rewards ≈ (Your Debt / Total Debt) × 50 RIPE

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
* **Duration Ratio**: 33% × 3-year max = 1-year lock
  * All auto-staked rewards are locked for 1 year
  * Ensures reward recipients become long-term stakeholders

**Why This Matters**: Rather than dumping tokens on the market, auto-staking creates a community of invested participants who earn governance power alongside their rewards. You're not just earning tokens — you're earning a voice in the protocol's future.

## Protocol Configuration

### Flexible Parameters

Mission Control governs all reward settings:

* **Emission Rate**: RIPE tokens minted per block
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
   * GREEN LP: 292.5 RIPE/day (65% of stakers)
   * RIPE LP: 67.5 RIPE/day (15% of stakers)
   * sGREEN: 45 RIPE/day (10% of stakers)
   * RIPE: 45 RIPE/day (10% of stakers)
3. Multiply: asset allocation × your percentage = daily rewards

**Example**: You have 1% of all staked RIPE → 45 × 1% = 0.45 RIPE per day = $4.50/day

### "What happens when I claim?"

The protocol enforces auto-staking parameters to align incentives:

**Current Settings (Protocol-Controlled):**

* **Auto-stake percentage**: 75% must be auto-staked
* **Lock duration**: 1 year (33% of max 3-year duration)
* **Your choice**: Only whether to claim now or wait

**Example Claim:**

* You have 1,000 RIPE rewards to claim
* Protocol requires 75% auto-stake with 1-year lock
* Result: 250 RIPE to your wallet + 750 RIPE locked in governance vault

**Why This Matters**: Auto-staking prevents market flooding and ensures reward recipients become long-term stakeholders with governance power. You're earning both tokens AND future influence!

### "Do different assets in the same pool compete?"

Yes, within each pool! For example, in the Stakers pool:

* RIPE stakers compete with other RIPE stakers
* RIPE LP stakers compete with other RIPE LP stakers
* But they share the total Stakers allocation based on configured percentages

### "What happens if nobody stakes/borrows?"

More rewards for those who do! If you're the only RIPE staker, you get 100% of RIPE's allocation in the Stakers pool. Early participants often see highest returns.

## The Early Bird Gets the RIPE

Right now, emissions are ramping up. TVL is growing. But it's still early.

Those juicy GREEN LP yields? They'll shrink when billions pour in. That insane RIPE staking APY? Only while the participant pool stays small.

Every block you wait is rewards you're not earning. Every day you delay is yield going to someone else.

The protocol pays those who show up. Are you showing up?

***

_For technical implementation details, see_ [_Lootbox Technical Documentation_](https://ripe-finance.gitbook.io/ripe-developers/treasury-and-rewards/lootbox)_._
