---
description: Buy Power, or Earn It?
---

# Governance: Buy Power, or Earn It?

Most protocols sell voting power to the highest bidder. Whale shows up, buys tokens, controls the protocol. Democracy at its finest.

Ripe rewards conviction over cash. Lock your tokens. Stack governance points as protocol blocks pass. A longer configured lock can raise the linear rate at which your position earns points.

Time in the market beats timing the market. Even in governance.

**The kicker**: Governance points account for both position size and time, so influence cannot be measured from wallet balance alone.

## Why Lock RIPE?

### The Power of Time Commitment

Traditional governance systems treat all tokens equally, allowing whales to buy influence at the last minute. Ripe's approach rewards genuine long-term believers:

* **Time-Weighted Power**: Governance points accumulate linearly based on normalized vault shares and elapsed protocol blocks
* **Lock Bonuses**: A configured multiplier can raise the point rate according to the position's remaining lock duration
* **Early Advantage**: Earlier deposits can accumulate points across more blocks
* **Aligned Incentives**: All else equal, a longer remaining lock raises the rate at which a position accumulates points

### Real Yield Through Staking

Beyond governance points, configured governance-vault positions can earn RIPE rewards:

* [**Block Rewards**](../earning-and-rewards/03-ripe-rewards.md): Eligible governance-vault positions can earn from the configured staker allocation
* **Enhanced LP Rewards**: Eligible governance assets can have different configured point weights
* **Auto-Staking**: Claim rewards into the core governance vault currently configured in Mission Control
* **Claim-Time Delivery**: Entitlement accrues through points; RIPE is minted and delivered when the user claims

## How Governance Points Work

### The Point Accumulation Engine

Your governance power grows through a sophisticated point system that rewards both commitment and patience:

```
Base Points = Normalized Vault Shares × Elapsed Protocol Blocks × Asset Weight
Lock Bonus = Base Points × Lock Bonus Multiplier
Total Points = Base Points + Lock Bonus
```

**Key Mechanics:**

* Points accumulate every block (not just at intervals)
* Longer locks multiply your earning rate
* Asset weights are configurable
* Points grow linearly; depositing more shares increases the position that earns future points

### Lock Duration Rewards

The vault can offer larger point-rate bonuses for longer remaining locks. The following values are a hypothetical configuration, not live parameters:

| Lock Duration     | Bonus Multiplier | Effective Point Rate |
| ----------------- | ---------------- | -------------------- |
| 1 day (minimum)   | 0%               | 1x points           |
| 3 months          | \~15%            | 1.15x points        |
| 6 months          | \~35%            | 1.35x points        |
| 1 year            | \~65%            | 1.65x points        |
| 2 years           | \~130%           | 2.3x points         |
| 3 years (maximum) | 200%             | 3x points           |

**Hypothetical example**: Under the table's assumed terms, locking 1,000 RIPE for 2 years starts with a 2.3x point rate relative to the assumed minimum lock. The boost declines with the lock time remaining.

### Asset Weighting System

Not all deposits need to have the same configured weight. For example, a deployment could use:

* **RIPE Tokens**: 100% weight (baseline)
* **RIPE LP Tokens**: 150% weight (50% bonus for liquidity provision)

This incentivizes users to provide liquidity while building governance power, strengthening the entire ecosystem.

## Managing Your Position

Each asset has one share-based position and one unlock block per user in the vault. A new deposit—including RIPE auto-staked during a reward claim—is converted into shares and blended with the existing position using the exact old/new share ratio and each portion's remaining lock. The resulting lock applies to the whole position for that asset, not only the newest deposit.

### Lock Extensions: Your Power Move

Extending locks is the ultimate governance strategy:

* **Updated Point Rate**: The remaining-lock bonus recalculates when the extension updates the position
* **Extension Only**: An explicit adjustment can extend the lock, not shorten it; the tradeoff is longer restricted liquidity
* **Share-Weighted Deposits**: A deposit can move the whole-position lock up or down through the share-weighted blend, while an explicit lock adjustment can only extend it
* **Flexibility**: Extend partially through lock period to optimize

**Pro Tip**: Check the resulting whole-position lock before extending or claiming rewards.

### Early Exit: The Nuclear Option

Need liquidity urgently? Early release may be available under the configured lock terms, but it is deliberately costly:

* **Configured Exit Fee**: The vault removes shares to apply the fee to the position's live claim
* **Another Holder Required**: Early release is unavailable when the user owns all shares for that asset; shares must remain after the release
* **Release, Then Withdraw**: Releasing clears the lock but does not transfer assets to the wallet; withdrawal is a separate action
* **Point Accounting**: Release does not instantly erase all stored governance points, but the share burn reduces future accrual and a later withdrawal reduces points with the shares withdrawn
* **Bad-Debt Protection**: If withdrawals for the asset freeze during bad debt, early release is blocked so the user does not pay to unlock a position that still cannot be withdrawn

**Hypothetical example**: If the configured exit fee were 80%, releasing a 10,000 RIPE-equivalent claim would leave approximately 2,000 RIPE-equivalent of withdrawable claim, subject to share rounding and the later withdrawal checks.

When the configured fee is nonzero, early release deliberately trades a portion of the live claim for access before expiry.

### Natural Unlock: Patience Rewards

When your lock expires:

* **Full Access**: Withdraw without an early-release fee, subject to ordinary vault restrictions
* **Continued Earning**: Points keep accumulating at base rate
* **Re-lock Option**: Establish new lock for bonus multiplier
* **Flexible Timing**: No rush — unlocked tokens remain productive

## Governance Points and RIPE Rewards

Governance-vault participation has two separate onchain accounting paths:

### 1. Governance Points (Future Voting Power)

* Accumulate continuously based on stake and time
* Lock bonuses multiply accumulation rate
* Grow linearly across elapsed blocks
* Contribute to voting weight under the governance rules

### 2. [RIPE Block Rewards](../earning-and-rewards/03-ripe-rewards.md) (Staker Allocation)

* Eligible governance assets can receive entitlement from the configured staker category
* Distribution uses the asset's accumulated category points and each user's accumulated balance points
* Auto-staked claims increase the share position used for future rewards
* Entitlement accrues automatically, but RIPE is minted and delivered only when a claim is processed

## What Governance Can Control

The governance system can be used to direct protocol control across:

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

Ripe uses accumulated governance points rather than a wallet-balance snapshot. Normalized shares, elapsed protocol blocks, the configured asset weight, and any remaining-lock bonus determine the point rate. More capital still matters; a longer lock raises the rate only with the other inputs held equal.

The protocol belongs to those who bet their future on it.

Power reflects both capital and sustained participation. The clock matters.

***

_Lock RIPE. Stack points. Own the future._

_For technical implementation details, see_ [_RipeGov Technical Documentation_](https://ripe-finance.gitbook.io/ripe-developers/vaults/ripegov)_._
