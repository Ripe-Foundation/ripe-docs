---
description: Participate in configured Stock Token and other collateral liquidation settlement
---

# Stability Pools: Participate in Stock Token and Collateral Settlement

Stability pools let deposited liquidity participate in configured liquidation settlement for eligible Stock Tokens and other collateral before auction fallback. Participation is passive after deposit, but routing, capacity, pricing, realized results, and any rewards remain conditional.

This is wholesale DeFi liquidations, democratized.

> **Examples, not live terms:** All assets, percentages, rates, and scenario values on this page are illustrative. See [RIPE Params](https://params.ripe.finance) for current onchain configuration.

## The Core Proposition

### Configured Liquidation Settlement Opportunities

After depositing, participants can share in eligible liquidation settlement without bidding on each event:

* **Passive Participation**: A deposit participates when the asset route, cohort compatibility, and capacity checks pass
* **Proportional Accounting**: Depositors hold shares in the cohort's valued custody and claims
* **Separate Transactions**: Deposits, claims, and withdrawals still require their own onchain transactions
* **Effective Settlement Spread**: The episode's effective spread can make the cohort supply less settlement value than the collateral's oracle value without guaranteeing profit

When a liquidation settles through the vault, settlement custody is consumed and the cohort receives collateral. Depositors continue to hold vault shares; active collateral claims enter cohort NAV through the configured oracle path. The realized result depends on the effective spread and subsequent asset prices.

### Multiple Potential Return Sources

Depending on configuration, stability pool participants can combine three distinct sources:

1. **Deposited-Asset Economics**: [sGREEN](01-sgreen.md) remaining in cohort custody retains exposure to changes in GREEN backing per share; settlement redeems and consumes the portion used
2. **Liquidation Settlement**: The cohort can receive collateral while supplying settlement value at the episode's effective spread
3. [**RIPE Rewards**](03-ripe-rewards.md): Can accrue protocol tokens only when the position and reward terms are configured

These sources have separate accounting and risks; none guarantees a positive return.

## How Stability Pools Work

### The Deposit Process

Ripe can configure different Stability-vault settlement assets. Common designs include:

**GREEN-pair LP Tokens**

* Retain exposure to any trading fees accrued under the LP's own pool mechanics
* Become eligible for RIPE rewards only after deposit when that reward path is configured
* Transfer to [Endaoment](../core-protocol/07-endaoment.md) treasury when consumed in ordinary Stability settlement

[**sGREEN**](01-sgreen.md) (Savings GREEN)

* Retains exposure to changes in GREEN backing per share while in the pool
* Redeemed and burned during liquidations

Your deposits are converted to shares representing your proportional claim on the pool's total value — including both deposited assets and accumulated liquidated collateral.

GREEN itself is not a Stability-vault deposit asset. It can appear as claimable custody after a redemption and is consumed and burned before the cohort's ordinary settlement asset in a later liquidation.

### The Liquidation Flow

When a borrower's position needs liquidation:

1. **AuctionHouse checks eligibility**: The collateral must permit Stability settlement and the vault must accept it as a claim asset
2. **The vault checks capacity**: It needs unreserved settlement custody, a usable price, and room for the claim asset
3. **Settlement follows the asset route**: Claimable GREEN is consumed first. GREEN and redeemed sGREEN are burned; other settlement assets transfer to EndaomentFunds
4. **Debt receives scaled credit**: Only the settlement value actually supplied, subject to rounding, reduces the liquidation target
5. **Auction fallback remains**: Any configured auction-eligible remainder can proceed to a Dutch auction

Example: If hypothetical Stock Token A has an oracle value of $100 and the configured spread is 10%, the pool supplies $90 of settlement value for one token. The $10 difference is an initial valuation spread, not guaranteed realized profit; later price movement, fees, and claim timing can change the outcome.

### USD Value-Based Accounting

Unlike simple token vaults, stability pools use sophisticated USD value-based share accounting:

* **Share Price = Total Pool Value / Total Shares**
* **Pool Value = Unreserved settlement-asset custody + Valued active claim assets**
* **Your Value = Your Shares × Current Share Price**

This ensures fair distribution regardless of which assets the pool holds at any moment.

Reserved claim custody cannot be spent as settlement liquidity. A small claim can remain dormant and directly claimable without entering active NAV until it meets the activation rules.

## The Economics of Liquidation Settlement

### How Configured Spreads Affect Pool Value

The episode's effective Stability spread determines the initial difference between collateral oracle value and the settlement value supplied by the pool. Per-asset liquidation-fee terms first contribute to the account's borrowing-power-weighted fee term; AuctionHouse then derives and can cap the effective spread for that liquidation episode. For example:

* **5% configured spread** = The pool supplies 95% of the collateral's oracle value
* **10% configured spread** = The pool supplies 90% of the collateral's oracle value
* **15% configured spread** = The pool supplies 85% of the collateral's oracle value

The percentages above are hypothetical effective spreads, not per-asset live terms. Oracle behavior, market movement, fees, and claim timing mean an initial valuation difference does not ensure profitability.

### Real-World Scenarios

**During Market Volatility**: Price swings may produce more eligible liquidations and settlement opportunities. The pool participates when its route and capacity checks pass, but the realized result remains market-dependent.

**In Stable Markets**: Fewer liquidations occur. Underlying asset economics and RIPE rewards continue only when their separate mechanisms and configurations provide them.

**Portfolio Effect**: As eligible liquidations settle across different collateral types, a cohort can build claims on several configured assets. That basket is not necessarily diversified, and settlement at a configured spread does not guarantee a below-market exit or positive return.

## Advanced Features

### Claiming Liquidated Collateral

After liquidations, you can claim your proportional share of accumulated collateral:

* **Flexible Claims**: Choose which assets to claim and when
* **Share Settlement**: Claiming burns the shares corresponding to the value delivered
* **Auto-Deposit Option**: Claimed assets can automatically enter Ripe deposit vaults
* **Batch Claims**: A bounded set of claim assets can be requested in one transaction
* **Optional RIPE Rewards**: A successful batch earns locked RIPE only when a rate is configured, allowance remains, and mint-and-deposit succeeds

**How to Claim:**

When claiming, you specify:
1. **Stability Pool Asset**: Which of your deposited assets to use (sGREEN or GREEN LP)
2. **Claim Asset**: Which available liquidated collateral you want to receive (a configured Stock Token, ETH, WBTC, etc.)
3. **Maximum USD Value**: Cap on how much to claim (or max for full claim)

Each result is capped by shares, claim custody, the requested maximum, and usable pricing. A batch is atomic and must transfer value for at least one requested asset.

**Delegation**: Others can claim on your behalf if you've granted `canClaimFromStabPool` permission in your delegation settings. This enables automated claim strategies.

#### Claim Incentives: Keeping Pools Healthy

When claim rewards are configured, the RIPE-per-dollar rate applies to aggregate USD value successfully claimed and is capped by the remaining rewards accounting allowance. Awarded RIPE is minted and deposited into the current core [governance vault](../governance-and-economics/02-governance.md) with the configured lock. Claiming does not replenish the original settlement asset.

### GREEN Redemption Mechanism

When redemption is enabled for the vault, asset, and recipient—and usable pricing and claim custody are available—an eligible caller can:

1. **Redeem GREEN for available collateral** using GREEN as a $1 input to the bounded calculation
2. **Help stabilize GREEN price** through arbitrage
3. **Change pool custody**: an sGREEN cohort deposits incoming GREEN into sGREEN, while other cohorts record it as GREEN claim custody

This can support GREEN-market arbitrage while changing the cohort's custody and risk exposure.

### Multi-Asset Accumulation

Over time, stability pools accumulate diverse collateral types:

* Configured Stock Tokens from compatible Stock Token-backed liquidations
* ETH from liquidated Ethereum positions
* cbBTC from Bitcoin-backed loans
* Various DeFi tokens from other collateral types
* GREEN from redemption operations

Your shares maintain exposure to the cohort's valued basket until claims or redemptions change it.

## Why Participate in Stability Pools?

### For Yield Seekers

* **Multiple potential return sources** from the deposited asset, liquidation settlement, and configured rewards
* **Passive participation** after deposit, with realized outcomes dependent on settlement and market conditions
* **Configured liquidation spreads** whose realized outcome remains market-dependent

### For Risk-Conscious Users

* **Pre-auction route** when the vault is selected and has capacity
* **Claim and withdrawal paths** subject to vault and protocol checks
* **Protocol protection** role enhances system stability

### For GREEN Ecosystem Supporters

* **Strengthen the protocol** by providing liquidation liquidity
* **Potentially earn while supporting** liquidation settlement, without a guaranteed return
* **Accumulate governance power** only when RIPE rewards are configured, earned, claimed, and deposited into governance

## The Liquidation Game, Simplified

Every market crash. Every overleveraged position. Every liquidation event.

Eligible liquidations can flow through a compatible Stability vault before auction fallback; incompatible, reserved, full, paused, unpriced, or illiquid routes are skipped.

Depositors do not need to bid on each liquidation, although deposits, claims, and withdrawals remain onchain transactions.

Deposit, let the shared vault provide conditional liquidation liquidity, and claim your proportional position when the applicable checks permit it.

***

_Provide liquidation liquidity without bidding on every auction._

_For technical implementation details, see the_ [_StabilityPool Technical Documentation_](https://ripe-finance.gitbook.io/ripe-developers/core-lending/stabilitypool)_._
