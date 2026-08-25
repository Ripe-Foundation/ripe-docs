---
description: Set It and Forget It Yield
---

# sGREEN: Set It and Forget It Yield

Your stablecoins are lazy. Sitting there. Doing nothing. Earning nothing.

Meanwhile, Ripe borrowers pay interest on loans backed by supported collateral. Deposit [GREEN](../core-protocol/01-green-stablecoin.md) into sGREEN to capture the configured share of borrower revenue directed to the savings vault—without a separate reward claim.

Accrued borrower interest can be realized for the protocol without waiting for each borrower to repay, as described below.

## Understanding sGREEN

sGREEN is GREEN's yield-bearing twin — a set-and-forget savings vault that automatically compounds protocol revenues into your holdings. Built on the battle-tested ERC-4626 standard, it transforms idle stablecoins into productive capital without the hassle of claiming rewards or managing positions.

## Where Your Yield Comes From

sGREEN backing can grow from borrower revenue directed to the savings vault:

1. **Borrower Interest Payments**: The configured share of accrued borrower interest directed to sGREEN
2. **Origination Fees (Daowry)**: When enabled, the configured fee on new borrowing can add value
3. **Unrealized Yield Flush**: Accrued interest is recorded as unrealized yield. A later borrow flushes that balance, mints the corresponding GREEN, and applies the configured revenue split; this is transaction-driven rather than a periodic autonomous job
4. **Other GREEN Inflows**: Any separately authorized GREEN transfer into the vault adds backing per share, but treasury allocation is not part of the automatic borrower-revenue path

**Note on Revenue Split**: Not all borrower revenue must go to sGREEN. The configured buyback portion is transferred as GREEN to the governance recipient, while the remainder increases sGREEN backing. Any RIPE market purchase is a separate governance operation, not an automatic part of the split.

The share conversion rate reflects the GREEN actually held by the vault; returns depend on realized protocol revenue and the configured split.

## How to Use sGREEN

### Option 1: Direct Deposit (Simplest)

Convert your GREEN to sGREEN with one transaction:

* Deposit a positive amount of GREEN that mints at least one share
* Receive sGREEN shares at the current exchange rate
* Hold without a vault maturity date; the share value reflects backing that subsequently reaches the vault

**Example**: At a 1.1 rate, depositing 1,100 GREEN gives you 1,000 sGREEN

### Option 2: Receive Borrowed GREEN as sGREEN

Advanced strategy for sophisticated users:

* All Ripe debt is denominated and accounted for in GREEN
* Choose whether the borrowed value reaches your wallet as GREEN or is converted into sGREEN
* You may also route the converted sGREEN directly into its configured [Stability Pool](02-stability-pools.md)
* Your GREEN debt continues to accrue under your borrowing terms while the sGREEN you receive follows the vault's share price

**Hypothetical example**: If a GREEN debt costs 5% APR and the received sGREEN earns 8% over the same period, the gross spread would be 3% before fees, compounding differences, and risk. These are example assumptions, not live rates.

### Option 3: Deploy to [Stability Pools](02-stability-pools.md) (Additional Exposure)

Stack multiple revenue streams by depositing sGREEN in stability pools:

* **Keep earning**: sGREEN base yield continues
* **Add liquidations**: Share in liquidation collateral acquired by the pool
* **Plus rewards**: Earn [RIPE tokens](03-ripe-rewards.md) when the Stability position is configured for rewards
* **Separate Sources**: Underlying yield, claim assets, and RIPE rewards have different accounting and realization paths

**Potential Return Sources**: sGREEN exchange-rate growth, realized liquidation outcomes, and configured RIPE rewards. None is guaranteed.

## How Accrued Borrower Interest Becomes Vault Backing

Borrower interest does not need to be individually repaid before it can enter the protocol's revenue calculation:

**How It Works**:

* Borrower interest accrues over elapsed time whether or not it has been repaid
* This interest is tracked but not yet "realized" as new GREEN
* A later eligible borrow calls the unrealized-yield flush
* New GREEN is minted to represent the flushed interest
* The configured buyback portion goes to governance and the remainder goes to sGREEN backing

**What This Means**:

* Realization does not require the original borrower to repay first, but it does require the later flush-triggering transaction
* Only the configured savings-vault portion increases sGREEN backing
* Standard borrower repayments and auction purchases burn the GREEN they apply to debt
* Stability settlement can instead transfer a configured non-GREEN asset for debt credit; GREEN and sGREEN settlement uses the GREEN-burning path

## The sGREEN Advantage

### How Value Accumulates Automatically

Unlike traditional staking where you claim rewards, sGREEN uses an elegant share-based system:

* **Set and Forget**: Your sGREEN balance is non-rebasing; its GREEN value increases when additional backing reaches the vault
* **No Gas Fees**: Value accrues through exchange rate appreciation, not token distributions
* **Revenue Through Backing**: GREEN directed to the vault raises assets per share without a separate holder claim
* **Share Accounting**: Ordinary deposits and withdrawals preserve the pro-rata conversion rate; GREEN revenue added without new shares raises it

**Hypothetical example**: Deposit 1,000 GREEN at a 1.0 rate → assume the rate reaches 1.08 after one year → your sGREEN is then worth 1,080 GREEN. These values are illustrative.

### True DeFi Flexibility

* **Vault Redemption**: Convert to GREEN subject to available backing and token/vault safety controls
* **No Time Lock**: The vault does not impose a maturity date, though pauses, blacklists, and backing checks can restrict exits
* **Transferability**: Send, trade, or use sGREEN subject to its token controls
* **Tax Treatment**: Depends on the user and jurisdiction; consult a qualified adviser

## Hypothetical User Scenarios

### Scenario 1: The Passive Saver

_Assume Sarah deposits 10,000 GREEN into sGREEN and the vault earns 7% over a year_

* Starting rate: 1.00 (10,000 sGREEN received)
* After 1 year at 7% APR: Rate is 1.07
* Sarah's value: 10,700 GREEN
* **Profit: 700 GREEN with zero actions taken**

### Scenario 2: The Yield Farmer

_Assume Alex deposits 50,000 GREEN worth of sGREEN into a Stability pool and realizes the example returns below_

* sGREEN base yield: 6% = 3,000 GREEN/year
* Liquidation profits: 8% = 4,000 GREEN/year
* RIPE rewards: 4% = 2,000 GREEN equivalent/year
* **Total return: 9,000 GREEN (18% APR) plus compounding**

### Scenario 3: The Borrower Who Chooses sGREEN Delivery

_Hypothetical: Jordan incurs 100,000 GREEN of debt and chooses to receive the proceeds as sGREEN_

* Assumed borrowing cost at 5%: 5,000 GREEN/year
* Assumed sGREEN earnings at 8%: 8,000 GREEN/year
* **Net profit: 3,000 GREEN/year on borrowed capital**

## Frequently Asked Questions

**Q: How does sGREEN compare to other yield-bearing stablecoins?** A: The difference isn't just sGREEN — it's the entire Ripe ecosystem:

* **Diverse Revenue Potential**: sGREEN can capture revenue from borrowing against the supported collateral portfolio
* **Unrealized Yield Flush**: Interest can be realized without the original borrower repaying, but only when a later eligible borrow triggers the flush and a nonzero savings allocation directs value to sGREEN
* **Triple Stack Potential**: A configured Stability position can combine sGREEN backing growth, liquidation claims, and RIPE rewards
* **Borrowing Activity**: Supported portfolio borrowing can generate interest and enabled fees; sGREEN receives only the configured savings share after that revenue is realized
* Other yield-bearing stables rely on narrow use cases or subsidized APYs. sGREEN's yield comes from solving a real problem: unified portfolio borrowing.

**Q: What drives sGREEN yields?** A: Borrower interest and enabled fees can create protocol revenue. sGREEN backing grows only when that revenue is realized and the configured savings share is directed to the vault, so returns are not guaranteed by loan volume alone.

**Q: Is there a minimum deposit?** A: The vault rejects a zero-amount deposit or one that would mint zero shares. Any interface minimum or other live restriction should be checked in the current transaction preview.

**Q: Can sGREEN be used as collateral?** A: An sGREEN position deposited in a Ripe Stability vault does not contribute borrowing power; Stability vaults are excluded from collateral-term calculations. Other configured vault roles or external protocols can treat sGREEN differently.

**Q: What happens during market crashes?** A: sGREEN remains exposed to GREEN's peg, smart-contract, backing, and liquidity risks. Market volatility can also change borrowing, repayment, and realized revenue.

**Q: How often does the exchange rate update?** A: sGREEN is non-rebasing. Its conversion rate reflects the vault's current GREEN assets and share supply whenever it is queried or used.

## Stop Leaving Money on the Table

Borrower interest accrues over time, and enabled origination fees can add protocol revenue. The configured share directed to sGREEN increases the GREEN backing its shares.

With sGREEN, the revenue directed to the savings vault is reflected in the position's exchange rate without a separate claim.

One deposit. Share-based revenue accrual without a separate reward claim.

Your stablecoins have been lazy long enough.

***

_Ready to start earning? Convert your GREEN to sGREEN through the Ripe Protocol interface._

_For technical implementation details, see the_ [_SavingsGreen Technical Documentation_](https://ripe-finance.gitbook.io/ripe-developers/tokens/savingsgreen)_._
