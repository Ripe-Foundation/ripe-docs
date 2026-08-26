---
description: Buy Power, or Earn It?
---

# Governance: Buy Power, or Earn It?

Most protocols sell voting power to the highest bidder. Whale shows up, buys tokens, controls the protocol. Democracy at its finest.

Ripe rewards conviction over cash. Lock your tokens. Stack governance points every block. That small holder who locked for 3 years? They outvote the whale who arrived yesterday.

**The kicker**: Onchain voting isn't live yet, but your points are already accumulating. Like mining Bitcoin in 2010.

> **Live terms live onchain.** Lock lengths, the maximum boost, asset weights, and the exit fee vary by deployment and change over time. Every number on this page is an example. [Params](https://params.ripe.finance) is the source of truth.

## Why Lock RIPE?

### The Power of Time Commitment

* **Time-Weighted Power**: Points accrue every block from stake size and remaining lock
* **Lock Bonuses**: Longer locks earn faster — up to 3x for a maximum lock
* **Early Advantage**: Points you earn today stay banked; nobody can buy them later
* **Aligned Incentives**: Those who lock longest have the most say

### Real Yield Through Staking

Beyond governance points, the vault earns [RIPE block rewards](../earning-and-rewards/03-ripe-rewards.md) from the staker category. RIPE LP earns more than plain RIPE, and claims stake straight back into the vault, so rewards compound. Reward points and governance points are tracked separately — locking earns you both, and the lock multiplier feeds into your reward share too.

## How Governance Points Work

### The Point Accumulation Engine

```
Base Points = Shares × Blocks Held × Asset Weight
Lock Bonus  = Base Points × Bonus Rate (from your remaining lock)
New Points  = Base Points + Lock Bonus  → added to your stored total
```

**Key mechanics:**

* Points accrue every block and are added to a running total
* Your bonus rate is read from your **remaining** lock each time your position is touched — a deposit, an extension, a withdrawal, or a reward claim that stakes into the vault — and applied to every block since the last touch
* So touch your position while the lock is still long to bank the high rate. A position left alone until after it unlocks earns **no bonus** for that whole stretch
* LP tokens get a 1.5x asset weight in the example

### Lock Duration Bonus

The bonus scales linearly from the minimum lock to the maximum. Example: 1-day minimum, 3-year maximum, 200% top boost.

| Remaining Lock    | Bonus  | Points Multiplier |
| ----------------- | ------ | ----------------- |
| 1 day (minimum)   | 0%     | 1x                |
| 3 months          | ~17%   | 1.17x             |
| 6 months          | ~33%   | 1.33x             |
| 1 year            | ~67%   | 1.67x             |
| 2 years           | ~133%  | 2.33x             |
| 3 years (maximum) | 200%   | 3x                |

**Example**: Lock 1,000 RIPE for 2 years and, while about 2 years remain, your position accrues points at 2.33x the rate of someone who locked for a day. Touch the position now and then to bank that rate; the bonus shrinks as the unlock approaches.

### Asset Weighting

* **RIPE**: 100% weight (baseline)
* **RIPE LP**: 150% weight in the example — a 50% bonus for providing liquidity

## Managing Your Position

You hold one blended position per asset in the vault. A new deposit blends its lock with what's already there, weighted by size: 1,000 RIPE with 2 years left plus a fresh 1,000 RIPE at 1 day gives one 2,000 RIPE position with about a year left. A big deposit at a short lock **shortens** your lock. Auto-staked rewards blend the same way, at the reward lock length.

### Lock Extensions: Your Power Move

* **One-Way**: You can push your unlock date later, never earlier
* **Banks First**: Pending points are settled at the old rate, then the new lock sets the rate going forward
* **Compound Strategy**: Extend when you claim rewards — the fresh stake lands on a long lock instead of shortening it

### Withdrawing

Withdrawing burns stored points in proportion to what you take out. Withdraw half, lose half your points; withdraw everything, lose them all.

A locked position is **not** shielded from your loan. If the vault asset backs your debt and you're liquidated or redeemed, the tokens are transferred out of your position, your points on them burn, and the recipient starts a fresh minimum lock.

### Early Exit: The Nuclear Option

Need liquidity before your unlock? If governance has turned on early exit for the asset, you can release the lock for a fee:

* **80% Fee** in the example: release 10,000 RIPE, keep ~2,000
* **Two Steps**: Release clears the lock; you withdraw separately, and that withdrawal burns your points
* **Needs a Nonzero Fee**: A zero fee means early exit is off, not free
* **Needs Company**: Someone else must still hold the asset in the vault
* **Bad-Debt Freeze**: If the protocol is carrying bad debt and the asset is set to freeze, release is blocked — you couldn't withdraw anyway

### Natural Unlock: Patience Rewards

When your lock expires, withdraw any amount with no fee. Whatever stays keeps earning base points, and you can re-lock any time for the bonus.

### If Governance Changes the Terms

If governance makes the terms worse — raises the exit fee on an asset that already had early exit, lowers the max boost, lengthens the minimum or maximum lock, or removes early exit — you get a courtesy unlock. It lands the next time your position is touched, so a quick touch frees you with no fee.

Governance can also permanently switch off point accrual for the whole vault or for one account; tokens stay withdrawable on the normal terms.

## What You'll Control (Coming Soon)

The governance module that will consume your points is a placeholder today; the points themselves are live, and your running total becomes your voting weight when onchain voting ships. RIPE holders will then control:

### Core Protocol Parameters

* **Risk Management**: Collateral ratios, liquidation thresholds, stability fees
* **Asset Integration**: New collateral approval, oracle selection, risk tiers
* **Economic Policy**: Interest rate models, fee structures, revenue distribution
* **System Limits**: Debt ceilings, deposit caps, exposure limits

### Treasury Operations

* [**Endaoment**](../core-protocol/07-endaoment.md) **Strategy**: Yield deployment across DeFi protocols
* **Partnership Terms**: Liquidity partnerships and revenue sharing
* **Capital Allocation**: Investment priorities and risk tolerance
* **Emergency Response**: Crisis management and recovery procedures

### Protocol Evolution

* **Technical Upgrades**: Smart contract improvements and new features
* **Incentive Design**: Reward distributions and emission schedules
* **Governance Rules**: Voting parameters and proposal requirements
* **Strategic Direction**: Long-term vision and growth initiatives

## Time to Choose Your Side

In most DeFi, whoever has the most money right now owns the protocol. Buy tokens, vote, dump tomorrow.

Ripe flips the script. Your weight = tokens × time locked × lock bonus. Can't buy your way to the top. Can't raid the treasury and run. Voting launches later. Power accumulates now.

***

_Lock RIPE. Stack points. Own the future._

_For technical implementation details, see_ [_RipeGov Technical Documentation_](https://ripe-finance.gitbook.io/ripe-developers/vaults/ripegov)_._
