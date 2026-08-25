---
description: Governance points, locks, and protocol administration
---

# Governance Points and Locking

Most protocols sell voting power to the highest bidder. Whale shows up, buys tokens, controls the protocol. Democracy at its finest.

Ripe's governance vault records conviction over time. Lock tokens, and the position can accumulate governance points across elapsed protocol blocks. A longer configured lock can raise the point rate applied when the position is checkpointed.

Time in the market beats timing the market. Even in governance.

**The kicker**: Governance points account for both position size and time, so they cannot be measured from wallet balance alone.

Governance points are an onchain accounting primitive, not authority to call administrative functions by themselves. The RipeGov vault and its Boardroom callback do not implement a token-holder proposal, vote, quorum, or execution flow. A separate compatible governance interface can use the points as voting weight. Protocol administration is executed by configured governor addresses through the applicable Switchboard, validation, and timelock paths, so locking a position alone does not create administrative rights.

## Why Lock RIPE?

### The Power of Time Commitment

Traditional governance systems treat all tokens equally, allowing whales to buy influence at the last minute. Ripe's approach rewards genuine long-term believers:

* **Time-Weighted Points**: Governance points are calculated from normalized vault shares and elapsed protocol blocks
* **Lock Bonuses**: A configured multiplier can raise the checkpointed point rate according to the position's remaining lock duration
* **Early Advantage**: Earlier deposits can accumulate points across more blocks
* **Aligned Incentives**: All else equal, a longer remaining lock raises the rate at which a position accumulates points

### Configured RIPE Rewards

Beyond governance points, configured governance-vault positions can earn RIPE rewards:

* [**Block Rewards**](../earning-and-rewards/03-ripe-rewards.md): Eligible governance-vault positions can earn from the configured staker allocation
* **Enhanced LP Rewards**: Eligible governance assets can have different configured point weights
* **Auto-Staking**: Claim rewards into the core governance vault currently configured in Mission Control
* **Claim-Time Delivery**: Entitlement accrues through points; RIPE is minted and delivered when the user claims

## How Governance Points Work

### The Point Accumulation Engine

The vault checkpoints points lazily when a relevant state-changing action updates the position. Read-only views can calculate the latest amount without storing it:

```
Base Points = Normalized Vault Shares × Elapsed Protocol Blocks × Asset Weight
Lock Bonus = Base Points × Lock Bonus Multiplier
Total Points = Base Points + Lock Bonus
```

This is the conceptual formula for one checkpoint interval. The contract applies integer scaling and rounding.

**Key Mechanics:**

* Elapsed blocks since the prior checkpoint determine the interval's base points
* The multiplier is calculated from the lock time remaining **at the checkpoint**, then applied to that entire uncheckpointed interval; the contract does not integrate a continuously declining multiplier block by block
* If the lock has expired by the time of the checkpoint, that interval receives no lock bonus
* The asset weight and lock terms used during a normal refresh can be the configuration present at the checkpoint, including for blocks before a configuration change
* Depositing more shares changes the position used for future intervals after the deposit checkpoint

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

**Hypothetical example**: Under the table's assumed terms, a checkpoint while 2 years remain on a 1,000 RIPE position would use the example 2.3x rate for the elapsed interval being recorded. At a later checkpoint, the then-remaining duration determines the multiplier for that later uncheckpointed interval. The table illustrates terms; it is not a live configuration.

### Asset Weighting System

Not all deposits need to have the same configured weight. For example, a deployment could use:

* **RIPE Tokens**: 100% weight (baseline)
* **RIPE LP Tokens**: 150% weight (50% bonus for liquidity provision)

This can direct more points toward configured liquidity positions while the weighting remains in effect.

## Managing Your Position

Each asset has one share-based position and one unlock block per user in a governance vault. A new deposit—including RIPE auto-staked during a reward claim—is converted into shares and blended with the existing position using the exact old/new share ratio and each portion's remaining lock. The resulting lock applies to the whole position for that asset, not only the newest deposit.

Auto-staked rewards and Reserve Engine auto-deposits resolve the current core governance vault through Mission Control. If the core pointer changes, a position in a historical governance vault remains a separate position; a later deposit into the current vault does not silently merge the two.

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
* **Nonzero Fee Required**: A configured zero exit fee disables early release; it does not make early release free
* **Another Holder Required**: Early release is unavailable when the user owns all shares for that asset; shares must remain after the release
* **Release, Then Withdraw**: Releasing clears the lock but does not transfer assets to the wallet; withdrawal is a separate action
* **Point Accounting**: Release does not instantly erase all stored governance points, but the share burn reduces future accrual and a later withdrawal reduces points with the shares withdrawn
* **Bad-Debt Protection**: If withdrawals for the asset freeze during bad debt, early release is blocked so the user does not pay to unlock a position that still cannot be withdrawn

**Hypothetical example**: If the configured exit fee were 80%, releasing a 10,000 RIPE-equivalent claim would leave approximately 2,000 RIPE-equivalent of withdrawable claim, subject to share rounding and the later withdrawal checks.

When early release is enabled and its configured fee is nonzero, it deliberately trades a portion of the live claim for access before expiry.

### Courtesy Unlock After Adverse Term Changes

The vault can clear an existing unlock without an exit fee when a later refresh sees specified live lock terms become worse for the position: early exit was removed, an already-available exit fee increased, the maximum lock boost fell, or the minimum or maximum lock duration increased. This protection is lazy. A deposit, withdrawal, lock adjustment, point update, or another refresh must touch the position while the worse terms are live; restoring the prior terms before that touch avoids the courtesy unlock, while restoring them afterward does not recreate the old lock.

A courtesy unlock clears the time restriction but does not withdraw the asset or bypass Teller pauses, bad-debt freezes, custody checks, or other withdrawal controls.

### Natural Unlock: Patience Rewards

When your lock expires:

* **Full Access**: Withdraw without an early-release fee, subject to ordinary vault restrictions
* **Continued Earning When Enabled**: An unlocked position can keep accumulating base points without a lock bonus while governance-point accrual remains enabled
* **Re-lock Option**: Establish new lock for bonus multiplier
* **Flexible Timing**: No rush — unlocked tokens remain productive

### Governance-Point Accrual Can Be Disabled

A Switchboard can irreversibly disable governance-point accrual for an entire RipeGov vault or for one user. Once disabled, later position updates do not calculate pending or future point accrual for the affected scope; there is no matching re-enable function. This control does not itself unlock or withdraw the deposited assets, whose ordinary vault restrictions remain separate.

## Governance Points and RIPE Rewards

Governance-vault participation has two separate onchain accounting paths:

### 1. Governance Points (Voting-Weight Primitive)

* Are recorded lazily from shares and elapsed blocks
* Use the remaining-lock multiplier evaluated at checkpoint time
* Can be supplied as voting weight to a compatible governance integration
* Do not themselves authorize protocol administration

### 2. [RIPE Block Rewards](../earning-and-rewards/03-ripe-rewards.md) (Staker Allocation)

* Eligible governance assets can receive entitlement from the configured staker category
* Distribution uses the asset's accumulated category points and each user's accumulated balance points
* Auto-staked claims increase the share position used for future rewards
* Entitlement accrues automatically, but RIPE is minted and delivered only when a claim is processed

## What Protocol Governance Can Configure

Configured governors can use the protocol's Switchboard and timelock routes to administer areas such as those below. Whether and how governance points select those governors or control a compatible voting interface is a separate integration from RipeGov's point accounting.

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
* **Registry Topology**: Governed contract addresses and integration routes
* **Lifecycle Controls**: Component availability, pauses, and bounded emergency actions

## What the Points Record

Ripe records accumulated governance points rather than relying on a wallet-balance snapshot. Normalized shares, elapsed protocol blocks, the asset weight, and the remaining-lock bonus evaluated at each checkpoint determine the added points. More capital still matters; a longer lock raises the checkpointed rate only with the other inputs held equal.

The accounting reflects both capital and sustained participation. The clock—and checkpoint timing—matter. Turning those points into proposal or voting rights requires a separate governance integration.

***

_Lock RIPE. Stack points. Understand how they can be used._

_For technical implementation details, see_ [_RipeGov Technical Documentation_](https://ripe-finance.gitbook.io/ripe-developers/vaults/ripegov)_._
