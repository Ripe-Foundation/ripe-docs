---
description: Set It and Forget It Yield
---

# sGREEN: Set It and Forget It Yield

Your stablecoins are lazy. Sitting there. Doing nothing. Earning nothing.

Meanwhile, Ripe borrowers pay fees and interest on loans backed by stock tokens, WETH, and the rest of their portfolios. That revenue can be yours. Deposit [GREEN](../core-protocol/01-green-stablecoin.md) into sGREEN and hold it. No staking. No claiming. No thinking.

> **Live terms live onchain.** Rates, fees, the revenue split, and which optional features are switched on vary by deployment and change over time. Every number on this page is an example. [Params](https://params.ripe.finance) is the source of truth.

## Understanding sGREEN

sGREEN is GREEN's yield-bearing twin: an ERC-4626 savings vault. Deposit GREEN, receive sGREEN shares. Your share balance never changes; what changes is how much GREEN each share is worth. When protocol revenue arrives in the vault, the exchange rate rises and every holder's position is worth more GREEN. No rebases, no reward claims, no gas spent collecting.

**Example:** Deposit 1,000 GREEN at a 1.00 rate and you hold 1,000 sGREEN. A year later the rate is 1.08. You still hold 1,000 sGREEN, now worth 1,080 GREEN. Zero actions required.

## Where Your Yield Comes From

Two revenue streams feed the vault:

1. **Origination fees.** Every new loan pays a small fee, taken out of the GREEN at the moment it's borrowed. (Borrowers using an Underscore Earn vault pay none.)
2. **Borrower interest.** Every loan accrues interest every second. How it reaches you is the part most people miss.

### How Interest Reaches You

Interest isn't paid to sGREEN in a separate step. Here's the loop:

* Every loan accrues interest every second, whether or not the borrower ever touches it.
* Whenever a loan is touched (a repayment, a new borrow, any update to the position), the interest accrued since last time is booked as "unrealized" protocol yield.
* The next time anyone borrows, the protocol mints all of that unrealized interest as new GREEN and sends sGREEN its share.

So sGREEN gets paid even if a given borrower never repays. The timing depends on the next borrow, not on that borrower. When loans are eventually repaid or liquidated, the GREEN used to pay them is burned, which keeps supply balanced.

**Revenue split.** By default, all of this revenue goes to sGREEN. Governance can route a share to itself instead, as GREEN, to fund RIPE buybacks. Buybacks are a separate step, not something the protocol does on its own. Whatever isn't routed away lands in the vault.

## How to Use sGREEN

### Option 1: Deposit GREEN

One transaction. Deposit any amount of GREEN that mints at least one share and you receive sGREEN at the current rate. No minimum beyond that. No lockup.

**Example:** At a 1.10 rate, depositing 1,100 GREEN gives you 1,000 sGREEN.

### Option 2: Borrow and Receive sGREEN

When you borrow, choose to receive the proceeds as sGREEN. Your debt stays in GREEN and keeps accruing at your borrow rate; the sGREEN you hold grows at the vault rate. If the vault rate is higher, the difference is yours.

**Example:** Borrow at 5% APR, hold sGREEN earning 8%: 3% net on the borrowed amount.

### Option 3: Deposit sGREEN into a [Stability Pool](02-stability-pools.md)

Put your sGREEN to work in a Stability Pool and you stack three income sources:

* **sGREEN yield** keeps accruing on whatever the pool hasn't spent yet
* **Liquidation proceeds**: the pool buys liquidated collateral at the liquidation spread, and you own your share
* **[RIPE rewards](03-ripe-rewards.md)** on your pool position

## Getting Out

* **Redeem any time.** Convert sGREEN back to GREEN in one transaction at the current rate. No delay, no penalty, no cooldown.
* **The blockers** are token-level pauses or blocklists on GREEN or sGREEN, plus the edge case of a vault left holding shares against no GREEN at all.
* **Transferable.** sGREEN is a standard ERC-20: send it, trade it, use it elsewhere.

## Three Scenarios

**The Passive Saver.** Sarah deposits 10,000 GREEN at a 1.00 rate and forgets about it. A year later, at 7% APR, the rate is 1.07. Her 10,000 sGREEN is worth 10,700 GREEN. 700 GREEN earned, zero actions taken.

**The Pool Depositor.** Alex puts 50,000 GREEN worth of sGREEN into a Stability Pool for a year:

* sGREEN yield: 6% = 3,000 GREEN
* Liquidation proceeds: 8% = 4,000 GREEN
* RIPE rewards: 4% = 2,000 GREEN equivalent
* **Total: 9,000 GREEN (18%), before compounding**

**The Carry Trade.** Jordan borrows 100,000 GREEN at 5% APR and takes it as sGREEN:

* Borrowing cost: 5,000 GREEN/year
* sGREEN earnings at 8%: 8,000 GREEN/year
* **Net: 3,000 GREEN/year on borrowed capital**

## Frequently Asked Questions

**Q: How does sGREEN compare to other yield-bearing stablecoins?** A: The yield comes from real borrowing. Ripe takes stock tokens, blue-chip crypto, stablecoins, and yield positions in one loan, so sGREEN captures interest and fees from a wide base of borrowers, not a subsidized APY from one narrow use case. And you can stack it: sGREEN in a Stability Pool earns liquidation proceeds and RIPE on top.

**Q: What drives sGREEN yields?** A: Borrowing demand. More loans mean more interest and fees. Fewer loans, less.

**Q: Is there a minimum deposit?** A: Only enough to mint one share. 10 GREEN or 10 million earn the same rate.

**Q: Can sGREEN be used as collateral?** A: Not for borrowing. sGREEN sitting in a Stability Pool adds nothing to your borrowing power. It is, however, the first thing [deleverage](../core-protocol/05-deleverage.md) spends to pay down your debt: your own sGREEN pool position is redeemed for GREEN and burned against what you owe. Think of it as a debt buffer, not collateral.

**Q: What happens during market crashes?** A: The exchange rate holds. Nothing takes GREEN out of the vault except holders redeeming their own shares, so falling prices don't touch it. Volatile markets often bring more borrowing, which means more interest and fees flowing in.

**Q: How often does the exchange rate update?** A: The moment revenue arrives. Origination fees land on every borrow; booked interest lands on the next borrow after it.

## Stop Leaving Money on the Table

Every second, Ripe borrowers accrue interest. Every new loan pays a fee. sGREEN catches it all and folds it into your balance while you sleep.

One deposit. Zero effort.

Your stablecoins have been lazy long enough.

***

_Ready to start earning? Convert your GREEN to sGREEN through the Ripe Protocol interface._

_For technical implementation details, see the_ [_SavingsGreen Technical Documentation_](https://ripe-finance.gitbook.io/ripe-developers/tokens/savingsgreen)_._
