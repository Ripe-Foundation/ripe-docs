---
description: Buy Power, or Earn It?
---

# Governance: Buy Power, or Earn It?

Most protocols sell voting power to the highest bidder. Whale shows up, buys tokens, controls the protocol. Democracy at its finest.

Ripe records conviction over time. Lock your tokens. Stack governance points block by block. A longer remaining lock can increase the rate at which an eligible position accrues points.

Time in the market beats timing the market. Even in governance.

Governance points are an onchain accounting primitive. How a proposal or voting system uses them depends on its governance integration; point accrual alone does not grant authority. All assets, percentages, durations, fees, tables, and scenarios on this page are illustrative; see [RIPE Params](https://params.ripe.finance) for current onchain configuration.

## Why Lock RIPE?

### The Power of Time Commitment

Traditional governance systems treat all tokens equally, allowing whales to buy influence at the last minute. Ripe's approach rewards genuine long-term believers:

* **Time-Weighted Power**: Governance points accumulate continuously based on both stake size and duration
* **Lock Bonuses**: A longer remaining lock can add a configured bonus to new point accrual
* **Early Advantage**: Each checkpoint adds newly accrued points to stored points; stored points do not compound
* **Integration-Dependent Influence**: Longer locks can accrue more points, while any downstream voting weight or authority depends on the configured governance integration

### Configured RIPE Rewards Through Staking

Beyond governance points, an eligible governance-vault position can accrue RIPE reward entitlement when the staker category, asset weight, emission rate, and allowance are configured:

* [**Block Rewards**](../earning-and-rewards/03-ripe-rewards.md): Entitlement uses separate Lootbox reward balance points and becomes RIPE only through a successful claim
* **Reward Asset Weighting**: Each eligible governance asset can have its own configured Lootbox staker weight
* **Claim-Time Staking**: A configured portion of a successful reward claim can be deposited into the current core governance vault
* **Separate Risks**: Locking constrains liquidity, and neither reward value nor governance authority is guaranteed

## How Governance Points Work

### The Point Accumulation Engine

Your governance power grows through a sophisticated point system that rewards both commitment and patience:

```
Base Points = Normalized Shares × Elapsed Blocks
Weighted Points = Base Points × Asset Weight
New Points = Weighted Points + Remaining-Lock Bonus
Stored Points = Previous Stored Points + New Points
```

**Key Mechanics:**

* Points accumulate every block (not just at intervals)
* Longer locks multiply your earning rate
* LP tokens receive 1.5x weight for providing liquidity
* Points accrue linearly over elapsed blocks and are added to the stored balance

### Lock Duration Rewards

The vault offers dramatic bonuses for longer commitments:

| Lock Duration     | Bonus Multiplier | Point Accrual Multiplier |
| ----------------- | ---------------- | ------------------------ |
| 1 day (minimum)   | 0%               | 1x points           |
| 3 months          | \~15%            | 1.15x points        |
| 6 months          | \~35%            | 1.35x points        |
| 1 year            | \~65%            | 1.65x points        |
| 2 years           | \~130%           | 2.3x points         |
| 3 years (maximum) | 200%             | 3x points           |

**Example**: Lock 1,000 RIPE for 2 years and earn 2.3x the governance points of someone who locks for just 1 day.

### Asset Weighting System

Not all deposits are created equal:

* **RIPE Tokens**: 100% weight (baseline)
* **RIPE LP Tokens**: 150% weight (50% bonus for liquidity provision)

This incentivizes users to provide liquidity while building governance power, strengthening the entire ecosystem.

## Managing Your Position

Each asset has one share-based position and unlock block per user in a governance vault. A new deposit is blended with the existing position using the exact old/new share ratio and each portion's remaining lock; the resulting lock applies to the whole position for that asset. Auto-staked rewards and Reserve Engine deposits resolve the current core vault through Mission Control, while a position in a historical vault remains separate if that pointer later changes.

### Lock Extensions: Your Power Move

Extending locks is the ultimate governance strategy:

* **Future Bonus**: Pending points checkpoint before extension; the new unlock affects later accrual
* **One-Way Extension**: The unlock can move later but not earlier through the extension route, increasing the remaining liquidity commitment
* **Additive Strategy**: Extensions change the rate for later points rather than multiplying stored points
* **Flexibility**: Extend partially through lock period to optimize

Position updates checkpoint the elapsed interval before applying the new lock.

### Early Exit: The Nuclear Option

Need liquidity urgently? Early release may be available under the configured lock terms, but it is deliberately costly:

* **Configured Exit Fee**: The vault burns shares to apply the fee to the position's live claim
* **Nonzero Fee Required**: A zero exit fee disables early release; it does not make release free
* **Another Holder Required**: Early release is unavailable when the user owns all shares for that asset
* **Release, Then Withdraw**: Release clears the lock; withdrawal is a separate action
* **Point Accounting**: Released shares reduce future accrual, but release does not instantly erase every stored point
* **Configured Bad-Debt Freeze**: When enabled, recorded protocol bad debt prevents release and withdrawal

**Example**: With an 80% configured fee, releasing a 10,000 RIPE-equivalent live claim leaves approximately 2,000 RIPE-equivalent before share rounding and later withdrawal checks.

This severe penalty ensures only genuine emergencies trigger early exits, protecting the governance system's integrity.

### Courtesy Unlock After Adverse Term Changes

A later position refresh can clear an existing unlock without a fee if early exit was removed, an already-available exit fee increased, the maximum lock boost fell, or the minimum or maximum lock duration increased. This protection is lazy: an update must touch the position while the worse terms are live, and it does not withdraw assets or bypass other controls.

### Natural Unlock: Patience Rewards

When your lock expires:

* **Lock Expires**: The time-based lock no longer prevents withdrawal; the separate withdrawal transaction still follows vault, pause, custody, and account controls
* **Continued Earning When Enabled**: Points keep accumulating at base rate
* **Re-lock Option**: Establish new lock for bonus multiplier
* **Flexible Timing**: No rush — unlocked tokens remain productive

### Governance-Point Accrual Can Be Disabled

A Switchboard can irreversibly disable point accrual for an entire RipeGov vault or one user. Later updates then calculate neither pending nor future accrual for that scope; this does not unlock or withdraw assets.

## Governance Points and RIPE Rewards

Governance-vault positions can combine two separate accounting systems:

### 1. Governance Points

* Accumulate continuously based on stake and time
* Lock bonuses multiply accumulation rate
* Add newly accrued points to the stored balance
* Feed the Boardroom hook, whose governance integration determines their downstream use

### 2. [RIPE Block Rewards](../earning-and-rewards/03-ripe-rewards.md) (Staker Allocation)

* Eligible positions accrue Lootbox reward balance points while the relevant reward paths are configured; these are separate from RipeGov governance points
* A successful claim is required before RIPE is minted or deposited into the governance vault
* Distribution combines the asset's staker points relative to global staker points with the user's reward balance points relative to that asset's balance-point total
* Claim settings determine the liquid and auto-staked portions; the claimant can also request that the full claim be staked

## How Governance Integrations Can Use Points

RipeGov supplies time-weighted points to the configured Boardroom hook. A governance integration can use those points in its voting or proposal logic, but holding RIPE or accumulating points does not itself grant contract authority. The contracts and roles installed for a deployment determine which decisions are governable, which can include:

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

Every protocol eventually faces the same question: Who really owns this?

In most DeFi, the answer is simple: whoever has the most money right now. Buy tokens, vote immediately, dump tomorrow. The mercenary model.

Ripe flips the script by recording sustained participation through additive, time-weighted points while locks constrain token mobility.

The protocol belongs to those who bet their future on it.

Governance authority still follows configured contracts and roles; a point balance does not bypass those controls.

***

_Lock RIPE. Stack points. Own the future._

_For technical implementation details, see_ [_RipeGov Technical Documentation_](https://ripe-finance.gitbook.io/ripe-developers/vaults/ripegov)_._
