---
description: Set It and Forget It Yield
---

# sGREEN: Set It and Forget It Yield

Your stablecoins are lazy. Sitting there. Doing nothing. Earning nothing.

Meanwhile, Ripe borrowers are paying interest on loans backed by everything from ETH to jpegs. A configured share of that revenue can increase sGREEN backing. Just deposit [GREEN](../core-protocol/01-green-stablecoin.md) into sGREEN. No staking. No claiming. No thinking.

Repayment is not the only realization event: later borrowing activity can flush previously accrued interest to protocol revenue.

> **Examples, not live terms:** Rates, fees, spreads, assets, and scenario values on this page are illustrative. See [RIPE Params](https://params.ripe.finance) for current onchain configuration.

## Understanding sGREEN

sGREEN is GREEN's yield-bearing twin — a set-and-forget savings vault that automatically compounds protocol revenues into your holdings. Built on the battle-tested ERC-4626 standard, it transforms idle stablecoins into productive capital without the hassle of claiming rewards or managing positions.

## Where Your Yield Comes From

Every sGREEN holder benefits from multiple real protocol revenue streams:

1. **Borrower Interest**: Active loans accrue interest into protocol accounting; realization into sGREEN backing follows the borrowing-triggered flush and configured revenue split described below
2. **Origination Fees (Daowry)**: The configured share of origination fees can add value
3. **Unrealized Yield Flush**: Accrued borrower interest is recorded as unrealized yield. When a later borrow succeeds, CreditEngine flushes that amount, mints the corresponding GREEN, and routes the configured revenue share to sGREEN. This is triggered by borrowing activity, not by a timer; without a triggering borrow, unrealized yield remains pending
4. **Future Treasury Yields**: [Governance](../governance-and-economics/02-governance.md) can direct [Endaoment](../core-protocol/07-endaoment.md) profits to sGREEN holders

**Note on Revenue Split**: Not all protocol revenue goes to sGREEN. Governance can direct a portion toward RIPE buybacks instead. When that's enabled, fees get split — some to sGREEN yield, some to buying RIPE off the market. Both tokens win when the protocol grows.

This isn't speculative yield — it's real revenue from actual protocol usage that compounds automatically into your position.

## How to Use sGREEN

### Option 1: Direct Deposit (Simplest)

Convert your GREEN to sGREEN with one transaction:

* Deposit any amount of GREEN to the vault
* Receive sGREEN shares at the current exchange rate
* Start earning immediately with no minimum or lock-up

**Example**: At a 1.1 rate, depositing 1,100 GREEN gives you 1,000 sGREEN

### Option 2: Receive Borrowed Proceeds as sGREEN

Advanced strategy for sophisticated users:

* Borrow GREEN and choose to receive the proceeds as sGREEN; the debt itself remains denominated in GREEN
* Your GREEN debt continues to accrue under its borrowing terms while the delivered sGREEN can change in GREEN value
* Profit when sGREEN yield exceeds your borrow rate

**The Math**: Borrow at 5% APR, earn 8% on sGREEN = 3% net profit on borrowed funds

### Option 3: Deploy to [Stability Pools](02-stability-pools.md) (Maximum Yield)

Stack multiple revenue streams by depositing sGREEN in stability pools:

* **Keep earning**: sGREEN base yield continues
* **Add liquidations**: Receive collateral through configured liquidation spreads
* **Plus rewards**: Earn [RIPE tokens](03-ripe-rewards.md) on top
* **Three potential return sources**: sGREEN backing growth, liquidation proceeds, and RIPE rewards are accounted through their respective mechanisms

**Potential Returns**: Base yield + liquidation profits + RIPE rewards = significant APR

## How Accrued Interest Is Realized

Repayment is not the only event that can realize accrued borrower interest. A later successful borrow can trigger the flush.

**How It Works**:

* Borrowers accumulate interest every block (paid or unpaid)
* This interest is tracked but not yet "realized" as new GREEN
* A successful borrow calls the unrealized-yield flush
* New GREEN is minted to represent the accrued interest
* The configured sGREEN share of that revenue increases vault backing

**What This Means**:

* Previously accrued interest can be realized without the original borrower repaying, when later borrowing triggers the flush
* Without a triggering borrow, unrealized interest remains pending rather than continuously updating sGREEN backing
* When borrowers eventually repay (or get liquidated), that GREEN is burned
* The system stays balanced while you keep earning

## The sGREEN Advantage

### How Value Accumulates Automatically

Unlike traditional staking where you claim rewards, sGREEN uses an elegant share-based system:

* **Set and Forget**: Your sGREEN balance stays constant while its GREEN value reflects backing per share
* **No Gas Fees**: Value accrues through exchange rate appreciation, not token distributions
* **Backing Growth**: Realized revenue directed to sGREEN increases GREEN backing per share
* **Monotonic Rate**: The exchange rate can only increase — mathematically impossible to decrease

**Real Example**: Deposit 1,000 GREEN at 1.0 rate → Wait one year → Rate becomes 1.08 → Your sGREEN now worth 1,080 GREEN. Zero actions required.

### True DeFi Flexibility

* **Vault Redemption**: Convert to GREEN subject to available backing and token or vault safety controls
* **No Maturity Date**: The vault does not impose a time lock, though pauses, blacklists, and backing checks can restrict exits
* **Transferability**: Send, trade, or use sGREEN subject to token controls
* **Tax Efficiency**: Value accrues through price appreciation, not taxable distributions

## Real User Scenarios

### Scenario 1: The Passive Saver

_Sarah deposits 10,000 GREEN into sGREEN and forgets about it for a year_

* Starting rate: 1.00 (10,000 sGREEN received)
* After 1 year at 7% APR: Rate is 1.07
* Sarah's value: 10,700 GREEN
* **Profit: 700 GREEN with zero actions taken**

### Scenario 2: The Yield Farmer

_Alex deposits 50,000 GREEN worth of sGREEN into stability pools_

* sGREEN base yield: 6% = 3,000 GREEN/year
* Liquidation profits: 8% = 4,000 GREEN/year
* RIPE rewards: 4% = 2,000 GREEN equivalent/year
* **Total return: 9,000 GREEN (18% APR) plus compounding**

### Scenario 3: The Arbitrageur

_Jordan borrows 100,000 GREEN at 5% APR and chooses sGREEN delivery_

* Borrowing cost: 5,000 GREEN/year
* sGREEN earnings at 8%: 8,000 GREEN/year
* **Net profit: 3,000 GREEN/year on borrowed capital**

## Frequently Asked Questions

**Q: How does sGREEN compare to other yield-bearing stablecoins?** A: The difference isn't just sGREEN — it's the entire Ripe ecosystem:

* **Diverse Revenue**: sGREEN captures yield from a protocol that can accept a broad range of supported collateral, including eligible NFTs, memecoins, and yield positions
* **Unrealized Yield Flush**: Later borrowing can realize previously accrued interest without waiting for the original borrower to repay
* **Triple Stack Potential**: Deploy sGREEN to stability pools for base yield + liquidations + RIPE rewards
* **Real Demand**: Borrowers use Ripe because eligible collateral across their portfolio can back one position — creating sustainable yield sources
* Other yield-bearing stables rely on narrow use cases or subsidized APYs. sGREEN's yield comes from solving a real problem: unified portfolio borrowing.

**Q: What drives sGREEN yields?** A: More borrowing can increase accrued interest and fees. Actual sGREEN yield depends on realization events and the configured revenue split.

**Q: Is there a minimum deposit?** A: No minimums. Whether you have 10 GREEN or 10 million, you earn the same percentage yield.

**Q: Can sGREEN be used as collateral?** A: An sGREEN position deposited in a Ripe Stability vault does not contribute borrowing power because Stability vaults are excluded from collateral-term calculations. Other configured vault roles or external protocols can treat sGREEN differently.

**Q: What happens during market crashes?** A: sGREEN continues to use backing-based share accounting, but market price, realized yield, and uninterrupted exits are not guaranteed during volatile conditions.

**Q: How often does the exchange rate update?** A: The live conversion rate reflects current GREEN backing per share when queried. Borrower-interest realization is not timer-based; it depends on a successful borrow triggering the unrealized-yield flush.

## Stop Leaving Money on the Table

Borrowing accrues interest and can generate configured fees; the share realized and directed to sGREEN can increase its backing.

With sGREEN, the configured share of realized protocol revenue can compound into your position through increased GREEN backing per share.

One deposit. Infinite compounding. Zero effort.

Your stablecoins have been lazy long enough.

***

_Ready to start earning? Convert your GREEN to sGREEN through the Ripe Protocol interface._

_For technical implementation details, see the_ [_SavingsGreen Technical Documentation_](https://ripe-finance.gitbook.io/ripe-developers/tokens/savingsgreen)_._
