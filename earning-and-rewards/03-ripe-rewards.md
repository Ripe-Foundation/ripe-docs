---
description: Get Paid to Use the Protocol
---

# RIPE Rewards: Get Paid to Use the Protocol

Every block. Every position. Every dollar borrowed or staked.

Ripe Protocol is watching. And paying.

250 million RIPE — a quarter of all supply — is set aside for the people who actually use the protocol, through block rewards and bonds. Not VCs. Not insiders. Users. Borrow GREEN? Get paid. Stake? Get paid more. Lock your rewards? Get paid even more.

> **Live terms live onchain.** The emission rate, category split, asset weights, auto-stake ratio, and lock terms vary by deployment and change over time. Every number on this page is an example. [Params](https://params.ripe.finance) is the source of truth.

## Quick Start: Understanding Your Rewards

**🎯 The One-Minute Version:**

```
YOUR ACTIVITY              →  CATEGORY (example split)  →  YOUR SHARE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Stake RIPE / RIPE LP       →  Stakers          90%      →  Your pool's slice × your points share
Deposit sGREEN / GREEN LP  →  Stakers          90%      →  Your pool's slice × your points share
Borrow GREEN               →  Borrowers        10%      →  Your borrow points ÷ all borrow points
Any other vault deposit    →  Depositors        0%      →  Dollar value × time (off until turned on)
Assets chosen by vote      →  Voters            0%      →  Community decides (off until turned on)

Points = balance × blocks.  Your share of a pool = your points ÷ that pool's points.
```

**💡 Key Insight**: You don't compete with every user — only with the people in your pool. A borrower never dilutes a staker. An sGREEN depositor never dilutes a RIPE LP staker.

## The Reward Engine

### Every Block Counts

Rewards don't arrive in weekly epochs. Each block adds a fixed amount of RIPE to the pot, drawn from a rewards allowance, and that RIPE is split across the four categories by their weights. The weights are normalized: 90/10/0/0 means stakers get 90% of every block's emission and borrowers get 10%.

Nothing is minted until you claim. Your entitlement accrues against the allowance; the tokens come into existence the moment you collect them.

Two other things draw on the same allowance: RIPE paid on [Stability Pool claims](02-stability-pools.md) (below), and, if governance has turned it on, a periodic distribution to Underscore wallet users.

### The Points System

```
Points = Balance × Blocks Held
```

That's the whole formula. A smaller position held longer can out-earn a whale's brief visit. It's democracy through mathematics.

Every vault-and-asset pair is its own pool with its own points total. The same token held in two different vaults earns in two separate pools.

## Reward Categories

### 1. Stakers (90% in the example) 💎

[**Governance Vault**](../governance-and-economics/02-governance.md) **(RIPE & RIPE LP)**

* **Lock Multiplier**: A longer lock boosts your points — up to 3x for a maximum-length lock. The boost is already inside your points share.
* **LP Advantage**: RIPE LP has its own asset weight, and it's the largest in the example.
* **Compounding**: Claims stake back into the vault, so rewards earn rewards.

[**Stability Pools**](02-stability-pools.md) **(**[**sGREEN**](01-sgreen.md) **& GREEN LP)**

* **Triple Yield**: sGREEN yield, liquidation proceeds, and RIPE on top.
* **No Lock Required**: Withdraw whenever the pool lets you.
* **Claim Rewards**: When you claim liquidated collateral from a pool, you earn extra RIPE per dollar claimed. That RIPE goes 100% into the governance vault with the reward lock — no liquid portion.

### 2. Borrowers (10% in the example) 💰

Borrow GREEN and your principal earns points every block:

* **Points**: Outstanding principal × blocks borrowed
* **Your Share**: Your borrow points ÷ all borrow points
* **Real Benefit**: RIPE earnings offset your interest

### 3. General Depositors (0% in the example)

Governance can turn on rewards for every other vault deposit, weighted by dollar value × blocks. Only assets that don't already earn as stakers qualify.

### 4. Voters (0% in the example)

Governance can direct rewards to specific assets by vote, steering liquidity where the protocol needs it.

## Understanding Your Share

### Asset Weights

Inside the stakers category, each asset carries a weight. Example:

* RIPE LP: 45
* GREEN LP: 25
* sGREEN: 15
* RIPE: 15

These are weights, not fixed percentages. Each pool accrues category points at its asset's weight every block, and a pool's slice of the category is its points ÷ all category points. With only these four pools and steady weights, the slices work out to 45% / 25% / 15% / 15%.

### How Rewards Actually Flow

```
Total RIPE Emissions (500 RIPE/day in the example)
            ↓
    ┌───────┴───────┐
    │ First Split  │ (by category weight)
    └───────┬───────┘
            ↓
    90% → Stakers
    10% → Borrowers
     0% → Depositors, Voters
            ↓
    ┌───────┴───────┐
    │ Second Split │ (by pool within the category)
    └───────┬───────┘
            ↓
    Each pool's slice = its points ÷ category points
            ↓
    Your share = your points ÷ that pool's points
```

Borrowers skip the second split: your borrow points ÷ all borrow points, done.

### Step-by-Step Calculation Guide

**🎯 Step 1: Which category?**

```
Staking RIPE, RIPE LP, sGREEN, or GREEN LP → Stakers (90%)
Borrowing GREEN                             → Borrowers (10%)
```

**🎯 Step 2: What's your pool's slice?**

```
500 RIPE/day at $10 = $5,000/day
Stakers: 450 RIPE/day    Borrowers: 50 RIPE/day

Within Stakers (450 RIPE/day):
- RIPE LP   45% = 202.5 RIPE/day
- GREEN LP  25% = 112.5 RIPE/day
- sGREEN    15% =  67.5 RIPE/day
- RIPE      15% =  67.5 RIPE/day
```

**🎯 Step 3: What's your share of the pool?**

```
Your Points = Balance × Blocks
Your Share  = Your Points ÷ Pool Points

Example: 1,000 RIPE staked for 100 blocks
- Your points: 1,000 × 100 = 100,000
- Pool points: 1,000,000
- Your share: 10%
```

**🎯 Step 4: Your rewards**

```
10% × 67.5 RIPE/day = 6.75 RIPE/day
At $10 per RIPE = $67.50/day
```

### Quick Reference Table

| Your Action      | Where You Earn            | How to Maximize                        |
| ---------------- | ------------------------- | -------------------------------------- |
| Provide RIPE LP  | Stakers (90%) · 45% slice | Larger position, held longer           |
| Provide GREEN LP | Stakers (90%) · 25% slice | Larger position, held longer           |
| Stake RIPE       | Stakers (90%) · 15% slice | Lock for 3 years (3x points)           |
| Deposit sGREEN   | Stakers (90%) · 15% slice | Stack with Stability Pool proceeds     |
| Borrow GREEN     | Borrowers (10%)           | Larger, longer loans                   |
| Other deposits   | Depositors (0%)           | Off until governance turns it on       |
| Voted assets     | Voters (0%)               | Off until governance turns it on       |

### Simple Rewards Estimator

Assumptions: 500 RIPE/day, RIPE at $10, the weights above. "Points share" means your points ÷ the pool's points — a position held the same length of time as everyone else's has a points share equal to its balance share.

**RIPE LP (45% of stakers):**

```
Daily RIPE ≈ Your points share of the RIPE LP pool × 202.5

Example: $100,000 in RIPE LP, 1% points share
= 1% × 202.5 = 2.025 RIPE/day = $20.25/day
= ~7.4% APR
```

**GREEN LP (25% of stakers):**

```
Daily RIPE ≈ Your points share of the GREEN LP pool × 112.5

Example: $100,000 in GREEN LP, 1% points share
= 1% × 112.5 = 1.125 RIPE/day = $11.25/day
= ~4.1% APR
```

**RIPE staking (15% of stakers):**

```
Daily RIPE ≈ Your points share of the RIPE pool × 67.5

The lock multiplier is already inside your points share:
10,000 RIPE with a 3-year lock counts like 30,000 unlocked.

Example: 10,000 RIPE ($100,000), 3-year lock, 3% points share
= 3% × 67.5 = 2.025 RIPE/day = $20.25/day
= ~7.4% APR, ~7.7% APY with daily compounding
```

**sGREEN (15% of stakers):**

```
Daily RIPE ≈ Your points share of the sGREEN pool × 67.5

Example: $100,000 in sGREEN, 2% points share
= 2% × 67.5 = 1.35 RIPE/day = $13.50/day
= ~4.9% APR
```

**Borrowing GREEN (10% of emissions):**

```
Daily RIPE ≈ Your borrow points ÷ all borrow points × 50

Example: $500,000 borrowed, 5% points share
= 5% × 50 = 2.5 RIPE/day = $25.00/day
= ~1.8% APR back against your interest
```

## Claiming Your Rewards

### What Happens When You Claim

1. The protocol tallies your points in every vault and asset you hold, plus your borrow points.
2. It works out your share of each category and mints that much RIPE.
3. **Stake All** puts the whole claim into the [governance vault](../governance-and-economics/02-governance.md) (that's also the contract's default if you call it directly).
4. Leave Stake All off and you take the liquid portion; the auto-stake ratio applies to the rest — 75% staked, 25% to your wallet in the example.

Auto-staked RIPE gets the reward lock: a ratio of the vault's lock range, 33% × 3 years ≈ 1 year in the example. It blends into your existing governance position, which can shift your unlock date — see [managing your position](../governance-and-economics/02-governance.md#managing-your-position).

**Example claim**: 1,000 RIPE owed. Stake All on: 1,000 RIPE locked in the governance vault. Stake All off: 750 RIPE locked, 250 RIPE to your wallet.

**Delegation**: Grant `canClaimLoot` and someone else can claim for you — useful for automated compounding.

**Deferred claims**: If a category you've earned in has nothing to pay out yet, your claim settles to zero and keeps your points. Nothing is lost; claim again later.

### Why Auto-Staking

Instead of dumping tokens on the market, auto-staking builds a community of invested participants who earn governance power alongside their rewards. You're not just earning tokens — you're earning a voice in the protocol's future.

## Common Questions About Rewards

### "Why do I calculate my share twice?"

Think of it as one calculation with two inputs: which pie you're eating from (your pool's slice of the category) and how big your slice is (your points share of the pool). It's a buffet where desserts are on one table and mains on another — you only compete with people at your table.

### "Do different assets in the same category compete?"

Only at the pool level. RIPE stakers compete with RIPE stakers for RIPE's slice; RIPE LP stakers compete with RIPE LP stakers for RIPE LP's slice. The slices themselves come from the asset weights.

### "What happens if nobody stakes or borrows?"

More for those who do. If you're the only RIPE staker, you get all of RIPE's slice. A category with no participants just holds its RIPE; it isn't redirected to another category.

## The Early Bird Gets the RIPE

Your share of a category is your points against everyone else's. Right now, everyone else is a small crowd. As deposits and borrowing grow, each participant's slice shrinks — the 7.4% in the examples above is a small-pool number.

The protocol pays those who show up. Are you showing up?

***

_For technical implementation details, see_ [_Lootbox Technical Documentation_](https://ripe-finance.gitbook.io/ripe-developers/treasury/lootbox)_._
