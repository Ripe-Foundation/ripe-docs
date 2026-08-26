---
description: Buy liquidated collateral at a configured spread
---

# Stability Pools: Buy the Dip at a Configured Spread

Forget hunting for dips. Forget timing the market. Forget competing with MEV bots.

Stability pools let deposited liquidity participate in configured liquidation settlement before auction fallback. Participation is passive after deposit, but routing, capacity, pricing, and any rewards remain conditional.

This is wholesale DeFi liquidations, democratized.

> **Examples, not live terms:** All assets, percentages, rates, and scenario values on this page are illustrative. See [RIPE Params](https://params.ripe.finance) for current onchain configuration.

## The Core Proposition

### Instant Arbitrage Opportunities

Unlike traditional liquidation systems that require active monitoring and complex bot infrastructure, stability pools democratize liquidation profits:

* **Passive Participation**: Deposit once and automatically participate in liquidations
* **Fair Distribution**: Profits shared proportionally among all depositors
* **No Technical Barriers**: No bots, no gas wars, no timing games
* **Configured Discounts**: Liquidation terms can quote collateral below its oracle value without guaranteeing profit

When a liquidation settles through the vault, settlement custody is consumed and the cohort receives a collateral claim. The realized result depends on the configured spread and subsequent asset prices.

### Multiple Revenue Streams

Depending on configuration, stability pool participants can combine three distinct sources:

1. **Base Asset Yield**: [sGREEN](01-sgreen.md) continues earning protocol revenue while in the pool
2. **Liquidation Premiums**: Settle collateral at a configured spread to oracle value
3. [**RIPE Rewards**](03-ripe-rewards.md): Earn protocol tokens when the position is configured for rewards

These sources have separate accounting and risks; none guarantees a positive return.

## How Stability Pools Work

### The Deposit Process

Ripe can configure different Stability-vault settlement assets. Common designs include:

**GREEN-pair LP Tokens**

* Earn trading fees while waiting for liquidations
* Become eligible for RIPE rewards only after deposit when that reward path is configured
* Transfer to [Endaoment](../core-protocol/07-endaoment.md) treasury when consumed in ordinary Stability settlement

[**sGREEN**](01-sgreen.md) (Savings GREEN)

* Continues earning base yield in the pool
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

Example: If ETH is worth $2,000 and the configured spread is 10%, the pool supplies $1,800 of settlement value for 1 ETH. The $200 difference is an initial valuation spread, not guaranteed realized profit.

### USD Value-Based Accounting

Unlike simple token vaults, stability pools use sophisticated USD value-based share accounting:

* **Share Price = Total Pool Value / Total Shares**
* **Pool Value = Unreserved settlement-asset custody + Valued active claim assets**
* **Your Value = Your Shares × Current Share Price**

This ensures fair distribution regardless of which assets the pool holds at any moment.

Reserved claim custody cannot be spent as settlement liquidity. A small claim can remain dormant and directly claimable without entering active NAV until it meets the activation rules.

## The Economics of Liquidation Profits

### How Liquidation Fees Become Your Profit

The protocol's liquidation fee structure directly determines your returns. When a position liquidates:

* **5% liquidation fee** = You buy collateral at 95% of market value
* **10% liquidation fee** = You buy collateral at 90% of market value
* **15% liquidation fee** = You buy collateral at 85% of market value

Actual spreads are configurable by collateral. Oracle behavior, market movement, and claim timing mean a quoted discount does not ensure profitability.

### Real-World Scenarios

**During Market Volatility**: Liquidations increase as prices swing, generating more profit opportunities. Your passive position captures value from market stress without active trading.

**In Stable Markets**: Fewer liquidations occur. Underlying asset economics and RIPE rewards continue only when their separate mechanisms and configurations provide them.

**Portfolio Effect**: As liquidations occur across different collateral types, you build a diversified basket of assets acquired at discount — essentially dollar-cost averaging into multiple positions at below-market prices.

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
* **Passive income** requiring no active management
* **Configured liquidation spreads** whose realized outcome remains market-dependent

### For Risk-Conscious Users

* **Pre-auction route** when the vault is selected and has capacity
* **Claim and withdrawal paths** subject to vault and protocol checks
* **Protocol protection** role enhances system stability

### For GREEN Ecosystem Supporters

* **Strengthen the protocol** by providing liquidation liquidity
* **Earn while protecting** the system from bad debt
* **Accumulate governance power** through RIPE rewards

## The Liquidation Game, Simplified

Every market crash. Every overleveraged position. Every liquidation event.

Eligible liquidations can flow through a compatible Stability vault before auction fallback; incompatible, reserved, full, paused, unpriced, or illiquid routes are skipped.

Depositors do not need to bid on each liquidation, although deposits, claims, and withdrawals remain onchain transactions.

Deposit, let the shared vault provide conditional liquidation liquidity, and claim your proportional position when the applicable checks permit it.

***

_Provide liquidation liquidity without bidding on every auction._

_For technical implementation details, see the_ [_StabilityPool Technical Documentation_](https://ripe-finance.gitbook.io/ripe-developers/core-lending/stabilitypool)_._
