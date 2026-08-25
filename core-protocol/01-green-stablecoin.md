---
description: The Stablecoin That Works Harder.
---

# GREEN: The Stablecoin That Works Harder

Stablecoin designs use different collateral, liquidity, and issuance models. GREEN is built around one account-level debt position backed by a portfolio of supported collateral. It also connects to [sGREEN](../earning-and-rewards/01-sgreen.md), [stability pools](../earning-and-rewards/02-stability-pools.md), and other configured mechanisms that work together to target the peg.

This is what happens when you stop asking "how do we make another stablecoin?" and start asking "how should stablecoins actually work?"

## Why GREEN Exists

### The Problem with Current Stablecoin Borrowing

Common lending designs make different tradeoffs:

**Isolated Collateral Positions**:

* Each collateral type can have a separate debt position
* Each position has its own terms and liquidation boundary
* A portfolio can require several loans to manage

**Shared Lending Markets**:

* Markets select their own supported collateral and risk terms
* Borrowing depends on available lender liquidity
* Liquidity and bad debt can be shared within a market

**Isolated Money Markets**:

* Terms can be tailored per market
* Each market has separate liquidity and collateral boundaries
* A multi-asset portfolio can still span several positions

### Ripe's Solution: Unified Multi-Collateral Borrowing

GREEN represents a fundamental rethink of stablecoin creation:

```
Traditional:  ETH → DAI Position 1
              WBTC → DAI Position 2
              USDC → Can't use

Ripe:         Multiple supported collateral assets
              ↓ (all combined)
              One GREEN Loan at Weighted Terms
```

Supported members of your portfolio can back a single GREEN loan. Collateral and debt are accounted per account rather than through a pooled lender balance sheet, while global debt limits, dynamic-rate signals, oracle and liquidity conditions, and protocol bad debt remain shared risks.

### Complementary, Not Competitive

When a yield-bearing token is registered with compatible custody, debt terms, and a usable price path, its external share or exchange-rate economics can continue while it is used as Ripe collateral. Illustrative token models include:

* **Aave-style receipt tokens**: Lending-market share value can remain reflected in the deposited token
* **Compound-style cTokens**: The external exchange-rate model can continue
* **Morpho-style positions**: A compatible vault can represent the yield-bearing position
* **Savings-rate tokens**: Their configured share-price mechanics can remain reflected while backing GREEN

These are architecture examples, not a current supported-asset list. Each asset still requires governance configuration, compatible vault behavior, and usable pricing; see [RIPE Params](https://params.ripe.finance).

### Built for the Entire Ecosystem

GREEN isn't just another stablecoin. It's the cornerstone of Ripe Protocol:

```
                        GREEN ECOSYSTEM
┌─────────────────────────────────────────────────────────┐
│                                                         │
│  BORROWING          YIELD              STABILITY        │
│  ┌─────────┐       ┌──────────┐       ┌──────────┐      │
│  │ Mint    │       │ sGREEN   │       │ Stability│      │
│  │ GREEN   │  ───> │ Backing  │  ───> │ Pools    │      │
│  │ Against │       │ per Share│       │ Earn     │      │
│  │ Assets  │       │ Can Grow │       │ Spreads  │      │
│  └─────────┘       └──────────┘       └──────────┘      │
│       │                  │                   │          │
│       └──────────────────┴───────────────────┘          │
│                          │                              │
│                    ┌─────▼──────┐                       │
│                    │  TREASURY  │                       │
│                    │  Endaoment │                       │
│                    │  Stabilizer│                       │
│                    └────────────┘                       │
│                                                         │
│  GREEN can participate in several configured mechanisms │
└─────────────────────────────────────────────────────────┘
```

## How GREEN Works

### Creation Through Borrowing

Borrower-originated GREEN is minted against value locked in Ripe Protocol:

1. **Deposit Collateral**: Lock any [supported asset](03-collateral-assets.md) (WETH, compatible tokenized assets, stablecoins, etc.)
2. **Borrow GREEN**: Mint new GREEN against your collateral
3. **Overcollateralized Borrowing**: Asset-specific LTV terms limit debt against the portfolio
4. **Pay Interest**: [Dynamic rates](02-borrowing.md#dynamic-interest-rates) that respond to market conditions
5. **Origination Fee (Daowry)**: A configured one-time fee can direct value to sGREEN and/or RIPE buybacks

**Hypothetical Multi-Asset Borrowing Example**:

```
Your Unified Portfolio:
- 5 WETH worth $10,000 (70% LTV = $7,000 borrowing power)
- 10,000 USDC (80% LTV = $8,000 borrowing power)
- 2 cbETH worth $4,000 (70% LTV = $2,800 borrowing power)
- 1M DEGEN worth $1,000 (40% LTV = $400 borrowing power)
- $50,000 of an ERC-20-compatible tokenized asset (40% LTV = $20,000 borrowing power)

Total Borrowing Power: $38,200 GREEN
Single Loan, Single Interest Rate, All Assets Working Together
```

### Standard Repayment with GREEN

When a borrower uses the standard GREEN repayment path:

1. **Send GREEN**: Return borrowed amount plus interest
2. **Burn the Payment**: The GREEN credited against debt is destroyed
3. **Restore Withdrawal Capacity**: A separate withdrawal can release collateral that is no longer needed to support the remaining debt

This standard borrow-and-repay path expands and contracts supply with borrowing activity. Separate department and collateral-settlement paths can reduce debt without burning a GREEN payment—for example, configured stable-side deleverage or non-GREEN Stability settlement. GREEN also has separately accounted mint-and-burn paths described on this page, including reserve-backed PSM conversion, realized borrower-revenue distribution, and authorized Endaoment liquidity operations.

### Cross-Chain Token Movement

On a configured CCIP route, outbound GREEN or RIPE delivered to its token-specific pool is burned on the source chain, and the corresponding token-specific pool mints to the recipient on the destination chain. Each pool exposes only its own token capability, while that deployment's RipeHq must separately authorize the pool to mint and can disable minting for that deployment. This describes the burn-and-mint mechanism; which routes and limits are active depends on each deployment's configuration.

## The Six Pillars of Stability

GREEN targets its $1 peg through multiple interconnected mechanisms. Some are automatic consequences of user actions, while others are optional or require an authorized transaction. Together, the redemption and conversion paths create arbitrage loops that can support the peg from multiple angles:

### 1. Overcollateralization Foundation

The bedrock of GREEN's stability:

* **Configured Collateralization**: Each asset contributes according to its governed debt terms
* **Extensible Asset Support**: Compatible vault and pricing modules can add different collateral types without changing the one-loan model
* **Portfolio Composition**: Multiple collateral assets can reduce concentration, but the protocol does not guarantee diversification or lower risk
* **Onchain Health Checks**: Relevant actions evaluate positions using current usable [price-oracle](06-price-oracles.md) outputs
* **Buffer Zones**: Multiple warning levels before [liquidation](04-liquidations.md)

### 2. Dynamic Interest Rate Response

When sustained, corroborated observations show GREEN overrepresented in the configured reference pool, borrowing rates can increase to encourage repayment. The contract uses that reserve-ratio history as its signal rather than reacting directly to a spot market price. For complete details, see [Dynamic Interest Rates](02-borrowing.md#dynamic-interest-rates) in the borrowing documentation.

**Normal Conditions**:

* Confirmed reference-pool observations remain below the configured danger threshold
* An ordinary borrower's next eligible debt checkpoint stores the weighted base rate for subsequent accrual
* Existing debt continues accruing at its previously stored rate until a protocol action updates the debt terms

**Below-Peg Response (GREEN Weak)**:

* Chronological pool snapshots measure whether GREEN remains overrepresented, rather than reacting to one observation
* Consecutive snapshots must corroborate an imbalance; qualifying intervals are weighted by their duration
* A ratio-based boost grows with the confirmed imbalance, while a duration-based boost grows with sustained danger
* The combined rate is bounded by a configured cap
* An eligible debt update checkpoints the resulting rate for subsequent accrual; a signal change does not retroactively reprice an elapsed interval
* Higher rates incentivize borrowers to repay, creating buying pressure for GREEN

**Above Peg Response (GREEN Strong)**:

* Normal base rates apply (no increase needed)
* An authorized, configured Endaoment action can mint GREEN within its pool-debt ceiling and add liquidity
* Additional sell-side liquidity can put downward pressure on the market price

**Recovery**: A single safe observation does not erase accumulated danger. Consecutive safe observations must cover the same configured freshness interval used to bound observations before the duration history resets. Mixed, stale, or unavailable intervals neither add danger credit nor count as confirmed recovery. A safe signal affects an existing borrower's stored rate only at a later eligible debt checkpoint. See [Dynamic Interest Rates](02-borrowing.md#dynamic-interest-rates) for the full mechanism.

### 3. Direct Redemption Mechanism

When their protocol, position, asset, recipient, and pricing checks pass, redemptions create an arbitrage path that can support GREEN below $1. For a detailed explanation of how this separate protective buffer works, see [The Redemption Buffer](04-liquidations.md#the-redemption-buffer) in the liquidations documentation.

**Dollar-Denominated Redemption**:

* GREEN is treated as a $1 debt-value input for sizing; this is protocol accounting, not a guarantee of GREEN's market price or literal dollar proceeds
* The inverse asset quote rounds down, and debt credit follows the amount credited by the vault rather than assuming the requested amount was delivered exactly
* Targets positions at or beyond the redemption threshold while they are not already `inLiquidation`; this can include a liquidation-eligible position before an episode starts
* Uses a bounded, health-restoring target rather than automatically emptying the selected borrower's debt
* Registered Underscore Earn-vault borrowers are skipped by ordinary credit redemption
* Execution is permissionless when the position, asset, recipient, prices, and protocol controls are eligible
* The resulting arbitrage opportunity can support GREEN's market price

**How Arbitrage Restores the Peg**:

```
GREEN trading at $0.97? Here's what happens:

1. Arbitrageurs spot the 3% discount
2. Buy 10,000 GREEN for $9,700 on the market
3. Submit a redemption sized for up to $10,000 of eligible collateral under the example accounting and rounding assumptions
4. Compare the $300 gross spread with execution costs and market risk
5. This buying pressure can support GREEN's return toward $1
```

**Important Constraint**: Redemptions require a position at or beyond its redemption threshold and stop once that position enters an active liquidation episode. During stable markets with healthy collateral ratios, direct redemptions may be unavailable. The protocol, asset, recipient, pricing, custody, and borrower-type checks must also pass.

### 4. Stability Pool Redemption Mechanism

A second powerful redemption path exists through [stability pools](../earning-and-rewards/02-stability-pools.md) that hold liquidated collateral:

**How Pool Redemptions Work**:

* Stability pools can accumulate configured collateral assets from liquidations
* GREEN is treated as a $1 accounting input when sizing the exchange for priced, claimable pool assets; inverse quotes and vault accounting can round the credited result
* **Availability Dependent**: The vault, asset, recipient, pricing, and claim custody must all qualify
* Creates arbitrage opportunities during periods of liquidation activity

**Important Constraint**: Pool redemptions require available claim collateral and enabled redemption configuration. A pool can reject or skip a request even when it has other claim assets.

**Complementary Redemption Paths**:

```
GREEN at $0.96? Potential redemption options:
1. Direct redemption (if positions in Redemption Zone)
2. Pool redemption (if liquidated collateral available)

Both paths subject to availability
```

**When Pool Redemptions May Be Available**:

* A compatible liquidation may add claim collateral to a configured Stability cohort
* Redemption still depends on enabled routes, usable pricing, active claims, cohort balances, conversion feasibility, and available custody
* Incompatible, reserved, full, or unfunded Stability routes fall back or decline rather than creating redemption capacity
* Market stress can create opportunities, but it can also reduce liquidity or make a route unavailable

### 5. Endaoment Treasury Operations

The [Endaoment](07-endaoment.md) is a protocol-owned treasury that can support the peg through authorized, configured transactions:

**Capital Arsenal from Bond Sales**:

* Accepted bond proceeds become treasury assets
* Treasury assets can enter configured yield or liquidity positions
* Assets can remain in custody until an authorized transaction deploys them
* Available reserves and configured permissions bound what an operation can do

**Authorized Stabilizer Transaction**:

```
Reference-pool adjustment:

Normalized paired-asset balance exceeds GREEN?
→ Use treasury GREEN first
→ Mint only the additional GREEN needed, within the pool-debt ceiling
→ Add a configured fraction of the imbalance as liquidity

Normalized GREEN balance exceeds the paired asset?
→ Remove a bounded amount of GREEN liquidity
→ Burn recovered GREEN up to the pool's recorded debt
→ Keep any remaining treasury value under the stabilizer's accounting checks
```

The balances are normalized into the same value scale before comparison. A Switchboard-authorized call performs the operation; the contract does not run an autonomous scheduler or force an exact 50/50 result in one transaction.

**Liquidity Management Powers**:

* **Multi-DEX Operations**: Deploy liquidity through configured Curve, Uniswap, Aerodrome, or other compatible routes
* **Concentrated Positions**: Use Uniswap V3 or Aero Slipstream for capital-efficient price support
* **Partner Programs**: Mint GREEN within configured debt limits to pair with compatible assets
* **Transaction-Based Rebalancing**: Authorized calls can adjust configured positions; the contract has no autonomous scheduler

**Strategic Market Making**:

* Can provide buy-side support when GREEN weak
* Can provide sell-side liquidity when GREEN strong
* Any proceeds or fees from configured positions remain protocol treasury value
* Operations are limited by assets in custody and the authority of the called route

**Why This Matters**:

* **Accounted Minting**: Authorized GREEN minting is bounded and tracked as pool debt
* **Configured Venues**: Operations can use compatible routes that governance has configured
* **Controlled Execution**: Authorized calls are bounded by configured pool, debt, and liquidity constraints
* **Treasury Retention**: Any resulting fees or proceeds remain under treasury accounting

The Endaoment transforms a passive treasury into an onchain market participant that can support GREEN's peg through governed operations.

### 6. Peg Stability Module (PSM)

When a direction is enabled, the PSM converts between GREEN and a reserve stablecoin around a $1 reference without using a DEX trade. The reserve token is fixed when that PSM is deployed and must use six decimals.

**Dead Simple**:

* **Mint GREEN**: Deposit the reserve stablecoin and receive GREEN, or request delivery through sGREEN when the output exceeds the contract's wrapping minimum; a smaller output is delivered as GREEN
* **Redeem GREEN**: Pay with GREEN or sGREEN and receive the reserve stablecoin
* **Independent Controls**: Minting and redemption can be enabled, limited, and allowlisted separately

**A Conditional Arbitrage Loop**:

```
GREEN trading below its reference?
1. Buy GREEN in the market
2. Redeem through the PSM if redemption is enabled and reserve capacity is available
3. If the conservative quote leaves a profit after fees, arbitrage buying can support the peg

GREEN trading above its reference?
1. Mint GREEN if minting is enabled and the applicable recipient, fee, and capacity checks pass
2. Sell GREEN in the market
3. If the quote leaves a profit after fees, arbitrage selling can support the peg
```

**Why It Works**:

* **No debt-position prerequisite** — the PSM is separate from collateral redemption
* **Direction-specific bounds** — ordinary minting is bounded by the caller's input, the conservative oracle-or-par quote, fees, and interval capacity; redemption is additionally bounded by reserve liquidity
* **Optional sGREEN handling** — mint proceeds can be delivered through sGREEN, and sGREEN can fund redemption
* **Optional reserve yield** — a configured yield position can deploy idle reserves and withdraw them when needed

**The Fine Print**:

Recognized-vault treatment is determined from the **recipient**, not merely the transaction caller:

* **Mint to an ordinary recipient**: Any enabled mint allowlist applies to the caller; the configured fee and interval capacity apply; output uses the lower of oracle value and par
* **Mint to a recognized Underscore vault**: The ordinary allowlist, fee, and interval controls are bypassed, but output uses the same conservative lower-of-oracle-and-par quote and remains bounded by the caller's supplied reserve token rather than by existing reserve inventory
* **Redeem to an ordinary recipient**: Any enabled redeem allowlist applies to the caller; the configured fee and interval capacity apply; output uses the lower of oracle value and par and cannot exceed available reserves
* **Redeem to a recognized Underscore vault**: The ordinary allowlist, fee, and interval controls are bypassed; output uses the more favorable higher-of-oracle-and-par quote but remains bounded by available reserves

See [RIPE Params](https://params.ripe.finance) for current configuration.

Bottom line: when configured and available, the PSM gives arbitrageurs a reserve-backed path that can help close a GREEN price gap.

## Additional GREEN Ecosystem Features

### Bad Debt Resolution

If bad debt exists during a [bond](../governance-and-economics/03-bonds.md) purchase, the oracle-valued payment can clear bad debt up to that value. Because the corresponding RIPE payout is accounted outside ordinary bond-distribution usage, authorized cap administration must count it separately toward RIPE's protocol-wide 1 billion-token cap across all blockchains.

### Protocol-Wide Integration

* GREEN is the only borrowable asset in Ripe Protocol
* Debt reduction is denominated in GREEN; Stability settlement can consume GREEN, sGREEN, or another configured Stability asset, while ordinary auction purchases pay GREEN
* Fee flows strengthen the ecosystem
* Designed for maximum composability with DeFi

### Keeper Network Protection

* Permissionless callers can monitor GREEN loans and submit liquidations when positions become eligible
* Productive first calls can earn a configured, bounded keeper reward
* Prompt execution can limit bad-debt exposure, but the contracts do not guarantee monitoring coverage or transaction timing

## GREEN Throughout Ripe Protocol

### Transform to sGREEN for Share-Based Savings

[sGREEN](../earning-and-rewards/01-sgreen.md) is GREEN's yield-bearing twin:

* **Set and Forget**: Deposit GREEN, receive sGREEN
* **Backing-Based Value**: The exchange rate can grow when configured revenue reaches the vault; sGREEN does not rebase
* **Protocol Revenue**: Captures the configured share of borrower interest and origination fees directed to sGREEN
* **Vault Redemption**: Redeem for GREEN subject to backing and the token/vault safety controls
* **Stability Compatibility**: A configured Stability vault can accept sGREEN as its settlement asset

### Participate in Stability Pools

Deploy a configured settlement asset such as sGREEN or a GREEN-pair LP token in [stability pools](../earning-and-rewards/02-stability-pools.md) for exposure to liquidation outcomes:

* **Multiple Return Sources**: Underlying asset economics, liquidation outcomes, and configured RIPE rewards
* **Settlement Spread**: Receive claimable collateral through the configured liquidation spread, with market and liquidity risk
* **Support Protocol**: Available, compatible deposits can supply liquidation settlement liquidity
* **Conditional Withdrawal**: Exits depend on unreserved custody and the ordinary token and vault controls

### Enable Borrowing Options

When taking loans, receive GREEN flexibly:

* **Direct GREEN**: Standard stablecoin delivery, subject to GREEN's token controls
* **Auto-sGREEN**: Receive sGREEN exposure directly; its backing per share can grow whenever additional GREEN reaches the savings vault, whether through configured revenue or a direct transfer
* **Stability Pool Entry**: Direct sGREEN delivery into the configured Stability vault

### Earn RIPE Block Rewards

Multiple ways to earn [RIPE rewards](../earning-and-rewards/03-ripe-rewards.md) through GREEN:

* **Borrowing GREEN**: Larger, longer-term loans earn more rewards
* **Stability Pool Deposits**: Eligible configured Stability positions can earn RIPE
* **Time-Weighted System**: Points accumulate based on position size × time
* **Offset Costs**: Rewards help reduce effective borrowing rates or boost stability pool returns

## GREEN and Other Stablecoin Designs

GREEN differs from several common issuance models:

* **Issuer-backed supply**: Existing stablecoin supply may be traded or lent by a market, but a user does not permissionlessly mint the issuer's token against that user's own portfolio
* **Isolated collateral positions**: Some designs separate debt by collateral type or market; GREEN combines supported collateral into one account-level debt position
* **Uncollateralized or reflexive supply**: These designs rely on different stabilization assumptions; GREEN's ordinary borrowing path is limited by configured collateral and debt terms
* **Optional savings path**: GREEN can be deposited into sGREEN, whose backing per share can grow when configured protocol revenue reaches the vault

**The Key Difference**: GREEN is built for borrowers who want to use a supported collateral portfolio efficiently. Different configured asset types can work together to back a single, manageable loan position.

## Why This Matters

Many stablecoin systems make users choose between isolated collateral, pooled lenders, yield, and simplicity.

GREEN combines supported collateral in one debt position, provides an optional sGREEN yield path, and uses complementary stability mechanisms to target its peg.

The mechanism's value is in that combination, subject to the configuration and risk conditions described throughout this guide.
