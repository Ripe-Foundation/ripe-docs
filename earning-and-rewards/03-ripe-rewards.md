---
description: Get Paid to Use the Protocol
---

# RIPE Rewards: Get Paid to Use the Protocol

Elapsed blocks. Eligible positions. A remaining rewards accounting allowance.

Ripe Protocol is accounting.

The Ledger tracks how much RIPE may still be allocated to rewards; it does not hold an escrowed pool of RIPE tokens. Configured emissions accrue category entitlement against that allowance. Borrow GREEN? You can earn from the borrower allocation. Stake in eligible pools? You can earn from their configured allocations. Lock your rewards? You can increase the position earning governance points.

When an allocation has fewer participants, each eligible participant may represent a larger share of its points.

## Quick Start: Understanding Your Rewards

**🎯 The One-Minute Version:**

The percentages and assets below are an illustrative configuration, not live protocol parameters. This example assumes the optional voter and general-depositor category weights are zero. If either is nonzero, the protocol normalizes all four raw category weights together, so the staker and borrower shares change. Later numeric examples use the same zero-optional-category assumption unless stated otherwise.

```
YOUR ACTIVITY          →  REWARD POOL  →  YOUR SHARE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Staking (All Types)    →  90% of RIPE  →  Based on asset type + position
  • RIPE/RIPE LP      →  Governance   →  Lock bonus up to 3x
  • sGREEN/GREEN LP   →  Stability    →  Size × time × weight
Borrowing GREEN        →  10% of RIPE  →  Based on debt size

OPTIONAL ALLOCATIONS:
General deposits       →  If configured  →  Based on eligible USD value
Voted assets           →  If configured  →  Based on configured points

SIMPLIFIED FLOW: Category rewards × asset point share × user point share
```

**💡 Key Insight**: Top-level categories are accounted separately. A borrower's points do not dilute the staker reward bucket.

## The Reward Engine

### Elapsed-Block Reward Accounting

Unlike traditional yield farming with discrete epochs, Ripe accounts for rewards using elapsed protocol blocks:

* **Elapsed Blocks Count at Checkpoint**: The blocks since the stored update and the configuration read at the next checkpoint determine new reward entitlement
* **Allowance-Capped Accumulation**: New entitlement cannot exceed the rewards accounting allowance still available
* **No Epoch Waiting Period**: Eligible points begin accruing once the position is included in reward accounting
* **Lazy Accounting**: User, asset, category, and global values update when their relevant protocol path checkpoints them; the mechanism does not promise manipulation-proof or timing-independent outcomes

RIPE is not pre-funded or minted to users every block. Points and category entitlement accrue first. A successful claim mints the calculated RIPE at that time, so it still depends on claim enablement, the Lootbox's RIPE mint authority, the RipeHq minting circuit breaker, token controls, and any required governance-vault deposit succeeding.

Configuration changes do not automatically split an uncheckpointed interval into old-config and new-config portions. At the next update, the contract applies the configuration it reads then across the elapsed blocks since the stored checkpoint. A governance process that intends a clean historical boundary must checkpoint the affected accounting before changing the configuration.

### The Points System

Ripe uses elapsed-point accounting rather than a current-balance snapshot:

```
User Balance Points = Eligible Position Rate × Elapsed Protocol Blocks
```

For ordinary eligible positions, the position rate comes from the vault's normalized balance or shares. For governance-vault positions, normalized shares and the remaining-lock bonus determine that rate. Separate asset-category points accrue from the configured asset weights. A claim uses both layers of accumulated points, so a current token-balance percentage is only a rough estimate when positions or configuration have changed.

## Reward Category Example

The following staker and borrower split uses that illustrative zero-optional-category configuration. Authorized governance paths administer the category weights stored in Mission Control.

### 1. Stakers (90% in This Example) 💎

Staking in protocol vaults earns the lion's share of rewards:

[**Governance Vault**](../governance-and-economics/02-governance.md) **(RIPE & RIPE LP)**

* **Base Rewards**: Size × time × asset weight
* **Lock Multiplier**: The configured remaining-lock terms can raise the point rate
* **Asset Weight**: RIPE and its LP token can have different configured point weights
* **Auto-Stake Strategy**: Deposited claims increase the shares that earn future points

[**Stability Pools**](02-stability-pools.md) **(**[**sGREEN**](01-sgreen.md) **& GREEN LP)**

* **Multiple Exposures**: Configured RIPE rewards plus liquidation claim outcomes
* **No Governance Lock**: Withdrawal liquidity still depends on unreserved settlement custody
* **Risk Buffer**: Help secure the protocol while earning

### 2. Borrowers (10% in This Example) 💰

Taking out GREEN loans earns rewards proportional to your debt:

* **Reward Basis**: Outstanding GREEN principal × time borrowed
* **Why It Matters**: Borrowing creates GREEN demand and protocol revenue
* **Point Effect**: Larger, longer-lived principal accumulates more borrower points while also increasing interest cost and liquidation exposure
* **Potential Benefit**: Claimed RIPE may offset part of borrowing costs; neither the reward amount nor its value is guaranteed

## Additional Configurable Reward Categories

### Vote Depositors

The configured voter category and asset weights can allocate rewards to specific deposits:

* **Configured Selection**: Only assets with the applicable nonzero voter-points weight participate
* **Targeted Incentives**: Configuration can direct entitlement toward selected liquidity
* **Separate from Voting Authority**: Reward-category naming does not itself grant a depositor proposal or execution rights

### General Depositors

May be activated to reward eligible vault deposits:

* **USD-Weighted**: Eligible underlying value supplies the general-depositor point rate
* **Category and Asset Gating**: The top-level general-depositor category must have an allocation, and an asset contributes general-depositor USD value only when its staker-points allocation is zero
* **Mutually Exclusive Funding Branch**: An asset with a nonzero staker-points weight does not simultaneously fund general-depositor points

## Understanding Your Share

### Asset-Specific Allocation Weights

Each supported asset has its own configuration that supplies relative weights inside the top-level reward categories:

* **Staker Points Weight**: The asset's configured weight relative to the total staker weights
  * Only applies to staked assets (RIPE, RIPE LP in [Governance Vault](../governance-and-economics/02-governance.md); [sGREEN](01-sgreen.md), GREEN LP in [Stability Pools](02-stability-pools.md))
  * Illustrative weights used in the examples below:
    * RIPE LP: 45%
    * GREEN LP: 25%
    * sGREEN: 15%
    * RIPE: 15%
* **Voter Points Weight**: The asset's configured weight relative to the total voter weights
  * Applies only when the asset has a nonzero voter weight
  * Weights do not need to sum to 100; the reward calculation normalizes each asset against the applicable total
  * For one asset, its configured staker and voter weights together cannot exceed 100%

**Important**: These are relative category weights, not guaranteed shares. In a hypothetical configuration where voter weights total 100 and Asset A has weight 50, Asset A receives half of the voter category. If the voter category itself receives 20% of total rewards, that example works out to 10% of total rewards.

### How Rewards Actually Flow (Simplified)

Think of RIPE rewards like a waterfall with two splits:

All amounts, splits, token prices, APRs, and returns in the following diagrams and calculations are hypothetical.

```
Total RIPE Emissions (500 RIPE/day)
            ↓
    ┌───────┴───────┐
    │ First Split  │ (by user type)
    └───────┬───────┘
            ↓
    90% → Stakers Pool
    10% → Borrowers Pool

    Vote & General Pools = 0% in this example
            ↓
    ┌───────┴───────┐
    │ Second Split │ (by asset within pool)
    └───────┬───────┘
            ↓
    Each asset gets a share based on
    accumulated category points
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

Assuming the example weights remain unchanged and there are no earlier outstanding points, within the Stakers pool (450 RIPE/day):
- RIPE LP tokens get 45% = 202.5 RIPE/day
- GREEN LP tokens get 25% = 112.5 RIPE/day
- sGREEN gets 15% = 67.5 RIPE/day
- RIPE tokens get 15% = 67.5 RIPE/day
```

**🎯 Step 3: What's YOUR Share of Your Asset Pool?**

```
Your Share = Your accumulated balance points ÷ Total accumulated balance points for that asset

Example: Your position accumulates 100,000 RIPE balance points
- Total RIPE balance points: 1,000,000
- Your share: 100,000 ÷ 1,000,000 = 10%
```

**🎯 Step 4: Calculate Your Rewards**

```
Your rewards = Your share × Asset's daily allocation

Example: 10% share of RIPE staking pool
10% × 67.5 RIPE/day = 6.75 RIPE per day
At the assumed $10 per RIPE = $67.50 of example daily entitlement
```

### Quick Reference Table

| Your Action              | Pool You Earn From        | How to Maximize                      |
| ------------------------ | ------------------------- | ------------------------------------ |
| Provide RIPE LP          | Stakers; 45% example weight | Accumulate a larger share of asset balance points |
| Provide GREEN LP         | Stakers; 25% example weight | Accumulate a larger share of asset balance points |
| Stake RIPE               | Stakers; 15% example weight | A longer remaining lock can raise the point rate |
| Deposit sGREEN           | Stakers; 15% example weight | Combine with Stability-pool outcomes when eligible |
| Borrow GREEN             | Borrowers (10% example)   | Larger, longer loans                 |
| General deposits         | When configured           | Based on eligible deposit points     |
| Voted assets             | When configured           | Based on eligible deposit points     |

### Simple Rewards Estimator

**"How much will I earn?"** - Quick formulas for common scenarios:

**Hypothetical assumptions for every example below:** 500 RIPE daily emissions at $10 per RIPE token. These are example inputs, not live parameters or price projections.

**For RIPE LP (45% example asset share):**

```
Daily Rewards ≈ (Your RIPE LP Balance Points / Total RIPE LP Balance Points) × 202.5 RIPE

Example: Your position has 1% of accumulated RIPE LP balance points
= 1% × 202.5 = 2.025 RIPE per day
= $20.25 per day
```

**For GREEN LP (25% of stakers):**

```
Daily Rewards ≈ (Your GREEN LP Balance Points / Total GREEN LP Balance Points) × 112.5 RIPE

Example: Your position has 1% of accumulated GREEN LP balance points
= 1% × 112.5 = 1.125 RIPE per day
= $11.25 per day
```

**For RIPE Staking (15% of stakers):**

```
Daily Rewards ≈ (Your RIPE Balance Points / Total RIPE Balance Points) × 67.5 RIPE

Example: After every participant's shares and remaining-lock rate are included, your position has 1% of accumulated RIPE balance points
= 1% × 67.5 = 0.675 RIPE per day
= $6.75 per day
```

Do not multiply this point share by a lock multiplier again. The lock already affects the rate at which both the user's balance points and the asset's aggregate balance-point denominator accumulated.

**For sGREEN Deposits (15% of stakers):**

```
Daily Rewards ≈ (Your sGREEN Balance Points / Total sGREEN Balance Points) × 67.5 RIPE

Example: Your position has 2% of accumulated sGREEN balance points
= 2% × 67.5 = 1.35 RIPE per day
= $13.50 per day
```

**For Borrowing GREEN (10% in this example):**

```
Daily Rewards ≈ (Your Borrow Balance Points / Total Borrow Balance Points) × 50 RIPE

Example: Your account has 5% of accumulated borrow balance points
= 5% × 50 = 2.5 RIPE per day
= $25.00 per day
```

## Auto-Staking Mechanism

### How Auto-Staking Works

Authorized governance paths configure the auto-staking settings stored in Mission Control:

* **Stake Ratio**: With **Stake All** off, the configured ratio is deposited and the remainder goes to your wallet
* **Stake All**: Turning it on deposits the entire claim instead of only the configured portion
* **Lock Duration**: The reward settings and current RIPE governance-vault terms determine the lock on the deposited portion
* **Current Vault Resolution**: The claim resolves the core governance vault from Mission Control instead of relying on a permanent vault ID

**Why This Matters**: Auto-staking sends the configured portion of a successful claim into the governance vault instead of the wallet. That deposited portion can earn governance points under the vault's current terms.

## Protocol Configuration

### Flexible Parameters

Mission Control stores the reward settings; authorized Switchboard and governor paths administer them:

* **Emission Rate**: RIPE entitlement accrued per protocol block
* **Category Weights**: Relative values normalized across the nonzero configured category allocations
* **Asset Allocations**: Individual asset point multipliers
* **Auto-Stake Settings**: Default ratios and durations

### Governance Evolution

Governance can adjust:

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

**Simple Method, using the same hypothetical values above:**

1. Find your pool's daily RIPE allocation:
   * Stakers total: 450 RIPE/day (90% of 500 RIPE)
   * Borrowers total: 50 RIPE/day (10% of 500 RIPE)
2. For stakers, find the asset's share of accumulated staker-category points. Under the unchanged example weights:
   * RIPE LP: 202.5 RIPE/day (45% of stakers)
   * GREEN LP: 112.5 RIPE/day (25% of stakers)
   * sGREEN: 67.5 RIPE/day (15% of stakers)
   * RIPE: 67.5 RIPE/day (15% of stakers)
3. Multiply the asset's category entitlement by your accumulated balance-point share for that asset

**Example**: You have 1% of the accumulated RIPE balance points → 67.5 × 1% = 0.675 RIPE per day = $6.75/day

### "What happens when I claim?"

The protocol applies the configured auto-staking parameters to align incentives:

**Configured behavior:**

* **Auto-stake percentage**: Authorized governance sets the minimum portion stored in Mission Control and deposited on a normal claim
* **Lock duration**: Derived from the reward settings and current RIPE governance-vault terms
* **Your choice**: Claim normally, or use **Stake All** to deposit the full claim

**The Full Claim Process:**

When you claim RIPE rewards, the Lootbox contract:
1. Iterates through ALL your vaults and assets to calculate total points
2. Determines your share of each reward pool (stakers, borrowers, etc.)
3. Calculates RIPE owed based on your points vs total pool points
4. Mints the RIPE owed by the completed calculation, subject to current mint authority, circuit-breaker, and token checks
5. Splits the claim according to the configured auto-stake ratio, unless **Stake All** is selected
6. Resolves the current core governance vault through Mission Control and deposits the staked portion for you

**Hypothetical Example Claim:**

* You have 1,000 RIPE rewards to claim
* Assume the protocol requires 75% auto-stake for this example
* With **Stake All** off: 250 RIPE goes to your wallet and 750 RIPE is deposited with the configured reward lock
* With **Stake All** on: all 1,000 RIPE is deposited

**Delegation**: Others can claim on your behalf if you've granted `canClaimLoot` permission. This enables automated compounding strategies.

**Why This Matters**: Auto-staking reduces the immediately liquid portion of a normal claim and places the configured portion into the governance vault. It does not guarantee market behavior or governance participation.

### "Do different assets in the same pool compete?"

Yes, within each pool! For example, in the Stakers pool:

* RIPE stakers compete with other RIPE stakers
* RIPE LP stakers compete with other RIPE LP stakers
* Their assets share the total Stakers allocation through accumulated category points whose rates come from configured weights

### "What happens if nobody stakes/borrows?"

Fewer outstanding points can mean a larger share for active participants. If you are the only RIPE staker and no other historical RIPE balance points remain outstanding, your position receives the user share of RIPE's staker-category entitlement.

## The Early Bird Gets the RIPE

When participation is low, each participant may represent a larger share of an allocation.

If an enabled allocation has fewer accumulated points competing for it, an eligible participant may receive a larger share. More participation, configuration changes, later checkpoints, or a depleted allowance can change that result.

The protocol rewards eligible activity only through the accounting and claim conditions described above. Are you showing up?
