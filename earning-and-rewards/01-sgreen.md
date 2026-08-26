---
description: Share in configured protocol revenue through GREEN backing per share
---

# sGREEN: Share in Configured Protocol Revenue

Your stablecoins are lazy. Sitting there. Doing nothing. Earning nothing.

Meanwhile, Ripe borrowers are paying interest on loans backed by supported Stock Tokens and other eligible collateral. A configured share of that revenue can increase sGREEN backing. Just deposit [GREEN](../core-protocol/01-green-stablecoin.md) into sGREEN. No staking. No claiming. No thinking.

A repayment can record newly accrued interest as pending unrealized yield, but it does not distribute that yield to sGREEN. A later successful borrow performs the flush and routes the configured share.

> **Examples, not live terms:** Rates, fees, spreads, assets, and scenario values on this page are illustrative. See [RIPE Params](https://params.ripe.finance) for current onchain configuration.

## Understanding sGREEN

sGREEN is GREEN's yield-bearing twin — an ERC-4626 savings vault whose GREEN backing per share can increase when configured revenue is actually transferred to it. The share-based design reflects that growth without rebasing or reward claims.

## Where Your Yield Comes From

sGREEN backing can increase from two implemented CreditEngine revenue sources:

1. **Origination Fees (Daowry)**: The configured share of the fee charged on a successful borrow is transferred to sGREEN
2. **Borrow-Triggered Interest Flush**: Accrued borrower interest is recorded as unrealized yield. When a later borrow succeeds, CreditEngine flushes that amount, mints the corresponding GREEN, and routes the configured share to sGREEN. Without a triggering borrow, unrealized yield remains pending

**Note on Revenue Split**: Not all of this revenue goes to sGREEN. The configured remainder is transferred in GREEN to governance for potential separately authorized buyback use. CreditEngine does not itself purchase RIPE.

Backing growth therefore depends on successful borrowing activity, the configured split, and revenue actually transferred to the vault.

## How to Use sGREEN

### Option 1: Direct Deposit (Simplest)

Convert your GREEN to sGREEN with one transaction:

* Deposit a nonzero amount of GREEN large enough to mint at least one sGREEN share
* Receive sGREEN shares at the current exchange rate
* Hold sGREEN at the current exchange rate with no vault-imposed maturity date; future backing growth depends on configured revenue

**Example**: At a 1.1 rate, depositing 1,100 GREEN gives you 1,000 sGREEN

### Option 2: Receive Borrowed Proceeds as sGREEN

Advanced strategy for sophisticated users:

* Borrow GREEN and choose to receive the proceeds as sGREEN; the debt itself remains denominated in GREEN
* Your GREEN debt continues to accrue under its borrowing terms while the delivered sGREEN can change in GREEN value
* Positive carry is possible when sGREEN backing growth exceeds your borrow rate

**Illustrative Math**: If borrowing costs 5% APR and sGREEN returns 8%, the gross positive carry is 3% before other costs and risks

### Option 3: Deploy to [Stability Pools](02-stability-pools.md) (Combined Return Sources)

Stack multiple revenue streams by depositing sGREEN in stability pools:

* **Keep exposure on unconsumed custody**: sGREEN that remains in the cohort continues to reflect revenue actually transferred to sGREEN; settlement redeems consumed sGREEN to GREEN and burns it
* **Add liquidations**: Receive collateral through configured liquidation spreads
* **Plus rewards**: Eligible configured positions can earn [RIPE tokens](03-ripe-rewards.md) on top
* **Three potential return sources**: sGREEN backing growth, liquidation proceeds, and RIPE rewards are accounted through their respective mechanisms

**Potential Return Sources**: sGREEN backing growth, liquidation proceeds, and configured RIPE rewards. Actual returns depend on protocol activity, configuration, and settlement outcomes.

## How Accrued Interest Is Realized

Debt updates, including repayment, can record newly accrued interest as pending unrealized yield. Distribution to sGREEN occurs only when a later successful borrow triggers the flush and routes the configured share.

**How It Works**:

* Borrowers accrue interest over elapsed time
* A debt update can record newly accrued interest as pending unrealized yield, but does not distribute it to sGREEN
* A successful borrow calls the unrealized-yield flush
* New GREEN is minted to represent the accrued interest
* The configured sGREEN share of that revenue increases vault backing

**What This Means**:

* Previously accrued interest can be realized without the original borrower repaying, when later borrowing triggers the flush
* Without a triggering borrow, unrealized interest remains pending rather than continuously updating sGREEN backing
* Standard repayment and GREEN-funded auction payments burn the applied GREEN; other configured settlement paths follow their own asset handling
* Configured revenue increases backing per share only when GREEN is successfully transferred into the vault

## The sGREEN Advantage

### How Backing per Share Can Increase

Unlike traditional staking where you claim rewards, sGREEN uses an elegant share-based system:

* **Set and Forget**: Your sGREEN balance stays constant while its GREEN value reflects backing per share
* **No Reward-Claim Transaction**: Backing growth appears in the exchange rate rather than through token distributions
* **Backing Growth**: Realized revenue directed to sGREEN increases GREEN backing per share
* **Proportional Vault Accounting**: Ordinary deposits and withdrawals preserve backing per share apart from rounding; incoming revenue can increase it

**Illustrative Example**: Deposit 1,000 GREEN at a 1.0 rate. If the rate later reaches 1.08, those shares represent 1,080 GREEN before costs or restrictions.

### True DeFi Flexibility

* **Vault Redemption**: Convert to GREEN subject to available backing and token or vault safety controls
* **No Maturity Date**: The vault does not impose a time lock, though pauses, blacklists, and backing checks can restrict exits
* **Transferability**: Send, trade, or use sGREEN subject to token controls

## Illustrative User Scenarios

### Scenario 1: The Passive Saver

_Sarah deposits 10,000 GREEN into sGREEN and forgets about it for a year_

* Starting rate: 1.00 (10,000 sGREEN received)
* After 1 year at 7% APR: Rate is 1.07
* Sarah's value: 10,700 GREEN
* **Illustrative backing increase: 700 GREEN, assuming the stated rate persists**

### Scenario 2: The Yield Farmer

_Alex deposits 50,000 GREEN worth of sGREEN into stability pools_

* sGREEN base yield: 6% = 3,000 GREEN/year
* Liquidation profits: 8% = 4,000 GREEN/year
* RIPE rewards: 4% = 2,000 GREEN equivalent/year
* **Illustrative total: 9,000 GREEN (18% APR), assuming all stated rates and rewards persist**

### Scenario 3: The Arbitrageur

_Jordan borrows 100,000 GREEN at 5% APR and chooses sGREEN delivery_

* Borrowing cost: 5,000 GREEN/year
* sGREEN earnings at 8%: 8,000 GREEN/year
* **Illustrative gross spread: 3,000 GREEN/year before other costs and risks**

## Frequently Asked Questions

**Q: How does sGREEN compare to other yield-bearing stablecoins?** A: The difference isn't just sGREEN — it's the entire Ripe ecosystem:

* **Diverse Revenue**: sGREEN captures yield from a protocol that can accept a broad range of supported collateral, including Stock Tokens, blue-chip crypto, yield positions, NFTs, and other eligible assets
* **Unrealized Yield Flush**: Later borrowing can realize previously accrued interest without waiting for the original borrower to repay
* **Three Potential Return Sources**: Deploy sGREEN to Stability pools for backing growth, liquidation proceeds, and configured RIPE rewards
* **Real Demand**: Borrowers use Ripe because eligible collateral across their portfolio can back one position — creating sustainable yield sources
* Other yield-bearing stables rely on narrow use cases or subsidized APYs. sGREEN's yield comes from solving a real problem: unified portfolio borrowing.

**Q: What drives sGREEN yields?** A: More borrowing can increase accrued interest and fees. Actual sGREEN yield depends on realization events and the configured revenue split.

**Q: Is there a minimum deposit?** A: The vault has no separately configured minimum, but a deposit must be nonzero and large enough to mint at least one share. All shares use the same backing-per-share rate; realized returns depend on revenue and configuration.

**Q: Can sGREEN be used as collateral?** A: An sGREEN position deposited in a Ripe Stability vault does not contribute borrowing power because Stability vaults are excluded from collateral-term calculations. Other configured vault roles or external protocols can treat sGREEN differently.

**Q: What happens during market crashes?** A: sGREEN continues to use backing-based share accounting, but market price, realized yield, and uninterrupted exits are not guaranteed during volatile conditions.

**Q: How often does the exchange rate update?** A: The live conversion rate reflects current GREEN backing per share when queried. Borrower-interest realization is not timer-based; it depends on a successful borrow triggering the unrealized-yield flush.

## Stop Leaving Money on the Table

Borrowing accrues interest and can generate configured fees; the share realized and directed to sGREEN can increase its backing.

With sGREEN, the configured share of realized protocol revenue can compound into your position through increased GREEN backing per share.

One deposit. Backing per share can grow without manual reward claims.

Your stablecoins have been lazy long enough.

***

_Ready to start earning? Convert your GREEN to sGREEN through the Ripe Protocol interface._

_For technical implementation details, see the_ [_SavingsGreen Technical Documentation_](https://ripe-finance.gitbook.io/ripe-developers/tokens/savingsgreen)_._
