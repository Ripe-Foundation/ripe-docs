---
description: The Stablecoin That Works Harder.
---

# GREEN: The Stablecoin That Works Harder

Your USDC sits there doing nothing. Your DAI requires a new vault for every asset. Your LUSD only accepts ETH.

GREEN? It's built different. Mint it against supported tokenized stocks and other eligible borrowing collateral across your portfolio. Deposit it into [sGREEN](../earning-and-rewards/01-sgreen.md) for potential backing growth. Use configured [stability pools](../earning-and-rewards/02-stability-pools.md) for potential liquidation proceeds. Multiple conditional mechanisms can support the peg.

This is what happens when you stop asking "how do we make another stablecoin?" and start asking "how should stablecoins actually work?"

## Why GREEN Exists

### The Problem with Current Stablecoin Borrowing

Traditional lending protocols force inefficient choices that limit how you can use stablecoins:

**Isolated Positions (MakerDAO/Liquity Model)**:

* Open ETH vault → Borrow DAI
* Open WBTC vault → Manage separate loan
* Add new asset? → Yet another position
* Result: Portfolio fragmentation, constant juggling

**Pooled Risk (Aave/Compound Model)**:

* Limited to "safe" assets only
* Your collateral backs everyone's loans
* One bad actor affects all users
* Result: Restricted innovation, systemic risk

**Isolated Money Markets (Morpho/Euler Model)**:

* Deposit ETH → One USDC loan
* Deposit WBTC → Another separate USDC loan
* Multiple positions to track and manage
* Each market needs lenders providing capital
* Result: Portfolio fragmentation AND rate inefficiency

### Ripe's Solution: Unified Multi-Collateral Borrowing

GREEN represents a fundamental rethink of stablecoin creation:

```
Traditional:  ETH → DAI Position 1
              WBTC → DAI Position 2
              USDC → Can't use

Ripe:         Stock Tokens + ETH + Stablecoins + Eligible Assets
              ↓ (eligible collateral combined)
              One GREEN Loan at Weighted Terms
```

Supported Stock Tokens and other eligible non-Stability collateral across your portfolio can back a single GREEN loan. Each asset contributes according to its configured terms while your risk remains isolated from other users.

### Complementary, Not Competitive

When a yield-bearing token and its Ripe vault path are supported and configured, its underlying economics can continue while it serves as collateral. Examples can include lending-market tokens, vault shares, and savings-rate tokens from integrations such as Aave, Compound, Morpho, or Maker. Support is asset- and deployment-specific; see [RIPE Params](https://params.ripe.finance) for current configuration.

### Built for the Entire Ecosystem

GREEN isn't just another stablecoin. It's the cornerstone of Ripe Protocol:

```
                        GREEN ECOSYSTEM
┌─────────────────────────────────────────────────────────┐
│                                                         │
│  BORROWING          YIELD              STABILITY        │
│  ┌─────────┐       ┌──────────┐       ┌──────────┐      │
│  │ Mint    │       │ sGREEN   │       │ Stability│      │
│  │ GREEN   │  ───> │ Auto-    │  ───> │ Pools    │      │
│  │ Against │       │ Compound │       │ Earn     │      │
│  │ Assets  │       │ Yield    │       │ Discounts│      │
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
│  Every GREEN serves multiple purposes simultaneously    │
└─────────────────────────────────────────────────────────┘
```

## How GREEN Works

### Creation Through Borrowing and Other Authorized Paths

Borrowing is the main user-facing GREEN creation path:

1. **Deposit Collateral**: Deposit a supported [Stock Token or other eligible asset](03-collateral-assets.md)
2. **Borrow GREEN**: Mint new GREEN against your collateral
3. **Configured Collateralization**: Asset-specific LTV terms determine borrowing power
4. **Pay Interest**: [Dynamic rates](02-borrowing.md#dynamic-interest-rates) that respond to market conditions
5. **Origination Fee (Daowry)**: A configured one-time fee flows through protocol revenue allocation

**Hypothetical Multi-Asset Borrowing Example**:

```
Your Unified Portfolio:
- Stock Token A worth $10,000 (60% LTV = $6,000 borrowing power)
- WETH worth $10,000 (70% LTV = $7,000 borrowing power)
- A stable asset worth $10,000 (80% LTV = $8,000 borrowing power)
- A yield position worth $4,000 (70% LTV = $2,800 borrowing power)

Total Borrowing Power: $23,800 GREEN
Single Loan, Single Interest Rate, All Assets Working Together
```

The assets and values above are illustrative. See [RIPE Params](https://params.ripe.finance) for current asset support and configuration.

Borrowing is not the only mint path. A successful borrow also realizes previously accrued system interest and mints that amount for configured revenue allocation. Other authorized mint paths use different controls: the PSM mints against received reserve assets under its conversion controls; the Endaoment stabilizer can mint GREEN as recorded pool debt up to its configured ceiling; and partner-liquidity minting is separately authorized and bounded by received partner-asset custody and usable oracle value.

Productive liquidation calls can also mint a bounded keeper reward: the keeper fee is added to borrower debt, and GREEN is minted to the keeper directly or wrapped into sGREEN.

### Destruction Through Repayment

Standard repayment burns the GREEN applied to debt:

1. **Send GREEN**: Return borrowed amount plus interest
2. **Burn Forever**: GREEN is permanently destroyed
3. **Reduce Debt**: Repayment can restore withdrawal capacity; withdrawing collateral is a separate permitted operation

Other configured routes can also burn GREEN, including redemption, Stability settlement, and Endaoment operations. Total supply therefore reflects borrowing, realized interest, keeper rewards, reserve and liquidity operations, repayment, and the other authorized mint-and-burn paths described on this page.

Standard repayment returns any payment above the live debt to the payer. A full payoff does not need to traverse or reprice the borrower's collateral. If a partial repayment cannot derive eligible replacement terms, the stored debt terms are preserved.

### Cross-Chain Token Movement

Configured CCIP pools move GREEN or RIPE with a burn-and-mint design: the token-specific source pool burns the amount sent, and the corresponding token-specific destination pool mints to the recipient. Each deployment must separately authorize its pool to mint, and route availability and limits remain deployment configuration.

## The Six Pillars of Stability

GREEN uses multiple interconnected stability mechanisms with different triggers and authorization paths. Its redemption and conversion mechanisms can create arbitrage loops that support the peg from multiple angles:

### 1. Overcollateralization Foundation

The bedrock of GREEN's stability:

* **Configured Ratios**: Collateralization depends on each asset's governed debt terms
* **Extensible Asset Support**: Different collateral types can be added when compatible custody, pricing, and risk configuration exist
* **Portfolio Effect**: When one asset drops, others may rise — true diversification
* **Action-Time Valuation**: Protocol actions evaluate account health using currently usable [price oracles](06-price-oracles.md)
* **Buffer Zones**: Multiple warning levels before [liquidation](04-liquidations.md)

### 2. Dynamic Interest Rate Response

When sustained reference-pool observations show GREEN overrepresented, borrowing rates can increase to encourage repayment. For complete details on how dynamic rates work, see [Dynamic Interest Rates](02-borrowing.md#dynamic-interest-rates) in the borrowing documentation.

**Normal Conditions**:

* The duration-weighted GREEN ratio remains below the configured danger trigger
* Borrowers pay their weighted base rates

**Below Peg Response (GREEN Weak)**:

* Snapshots are evaluated chronologically, with each valid interval weighted by its duration
* The lower ratio of each interval's two endpoints is used, so one isolated high observation cannot raise the signal
* Stale, out-of-order, or excessively gapped history fails closed instead of being extrapolated through a live tail
* Configured ratio and duration boosts can increase the rate, subject to the configured cap
* Higher rates incentivize borrowers to repay (buy GREEN)
* Repayment can create buying pressure that helps move GREEN toward its target

**Above Peg Response (GREEN Strong)**:

* Normal base rates apply when the reference-pool signal is below the configured trigger
* Endaoment can mint GREEN to add liquidity
* Increased supply can create selling pressure that helps move GREEN toward its target

**Confirmed Recovery**: Mixed safe/danger intervals do not erase accumulated danger duration. Consecutive safe endpoints must cover the configured freshness window before that history resets.

### 3. Direct Redemption Mechanism

Direct redemption provides a conditional arbitrage path that can support GREEN when it trades below $1. It is not an automatic restoration mechanism or guaranteed price floor. For a detailed explanation of how redemptions work as a protective buffer, see [The Redemption Buffer](04-liquidations.md#the-redemption-buffer) in the liquidations documentation.

**$1 Accounting and Rounding**:

* GREEN is treated as a $1 debt-value input when sizing a redemption; the inverse asset quote rounds down, and debt credit follows the amount actually credited by the vault
* Targets only positions in the "Redemption Zone" (at or below the redemption threshold)
* Requires a submitted transaction and the applicable enablement, recipient, borrower, valuation, backing, and delivery checks
* Can create buying pressure, but does not guarantee restoration of GREEN's market price

**Illustrative Potential Arbitrage**:

```
GREEN trading at $0.97? Here's what happens:

1. Arbitrageurs spot the 3% discount
2. Buy 10,000 GREEN for $9,700 on the market
3. Submit a redemption sized for up to $10,000 worth of eligible collateral
4. Potential gross spread: $300 before fees, slippage, execution risk, and collateral-price changes
5. A successful redemption can add buying pressure that helps GREEN move toward $1
```

**Important Constraint**: Redemptions are only possible when positions exist in the Redemption Zone and the relevant asset, recipient, pricing, backing, and route checks pass. During stable markets with healthy collateral ratios, direct redemptions may be unavailable.

When those conditions hold, the mechanism can provide buying pressure as positions move toward liquidation.

### 4. Stability Pool Redemption Mechanism

A second powerful redemption path exists through [stability pools](../earning-and-rewards/02-stability-pools.md) that hold liquidated collateral:

**How Pool Redemptions Work**:

* Stability deposits are represented by shares whose NAV includes unreserved settlement assets and strictly priced active claim assets
* Small liquidation claims can remain dormant until they qualify for activation; active claims can later retire under bounded residual rules
* Public claims and redemptions use batch routes and require usable pricing, available claim custody, and unreserved settlement liquidity
* A compatible liquidation can fall back to an ordinary auction when Stability settlement is unavailable
* Can create arbitrage opportunities during periods of liquidation activity when the redemption checks pass

**Important Constraint**: Pool redemptions require the stability pools to hold collateral from recent liquidations. During calm markets with no liquidations, this redemption path may be unavailable.

**Complementary Redemption Paths**:

```
GREEN at $0.96? Potential redemption options:
1. Direct redemption (if positions in Redemption Zone)
2. Pool redemption (if liquidated collateral available)

Both paths subject to availability
```

**When Pool Redemptions Are Most Effective**:

* **Market Stress**: Eligible liquidations can add collateral custody to compatible pools
* **Price Volatility**: More positions may reach liquidation eligibility, subject to submitted transactions and route checks
* **Claim Availability**: Compatible settled collateral can create conditional redemption capacity
* **Peg Support**: Eligible submitted redemptions can add GREEN demand when that capacity is available

### 5. Endaoment Treasury Operations

The [Endaoment](07-endaoment.md) serves as GREEN's financial fortress — a protocol-owned treasury with vast capabilities to defend the peg:

**Capital Arsenal from Bond Sales**:

* Bond proceeds transfer the configured payment asset to EndaomentFunds treasury custody for later authorized use
* Treasury assets can be deployed through authorized DeFi operations
* Available resources depend on bond activity, treasury performance, existing custody, and authorized execution

**Switchboard-Authorized Stabilizer Action**:

A Switchboard-authorized caller submits each stabilizer action; the Endaoment does not run on an autonomous scheduler.

```
Normalized pool-balance adjustment:

Normalized GREEN balance exceeds the paired asset?
→ Remove excess GREEN liquidity
→ Burn GREEN tokens permanently
→ Adjust the pool balance in a direction that can support price

Normalized paired-asset balance exceeds GREEN?
→ Mint new GREEN (tracked as debt)
→ Add liquidity to deepen markets
→ Adjust the pool balance in a direction that can support price
```

**Liquidity Management Powers**:

* **Configured Venue Operations**: Each authorized action targets one registered integration and pool; multiple venues require separate actions
* **Concentrated Positions**: Configured concentrated-liquidity integrations can provide capital-efficient price support
* **Partner Programs**: Mint GREEN paired with partner assets for deeper markets
* **Transaction-Based Rebalancing**: Move capital between pools through authorized transactions

**Strategic Market Making**:

* Can provide buy-side support when GREEN weak
* Can provide sell-side liquidity when GREEN strong
* Realized proceeds from authorized market operations remain treasury assets
* Treasury assets and realized yield can fund future operations

**Why This Matters**:

* **Deep Resources**: An authorized stabilizer action can mint GREEN within the configured pool debt ceiling
* **Authorized Execution**: Each operation requires a Switchboard-authorized transaction and must pass configured controls
* **Potential Treasury Growth**: Successful operations can generate proceeds that remain with the treasury

The Endaoment transforms from passive treasury to active market participant, with authorized operations that can support GREEN's peg.

### 6. Peg Stability Module (PSM)

Forget complex arbitrage strategies. When a direction is enabled, the PSM converts between GREEN and its reserve stablecoin around a $1 reference. The reserve token is immutable for that PSM deployment and must use six decimals.

**Dead Simple**:

* **Mint GREEN**: Deposit the reserve stablecoin, receive GREEN around the 1:1 reference (minus a fee if applicable)
* **Redeem GREEN**: Burn GREEN, receive the reserve stablecoin around the 1:1 reference (minus a fee if applicable)
* **Reserve Backed**: Redemption is separate from danger-zone collateral and Stability claims, but still depends on enabled configuration and available reserves

**Illustrative, Conditional Arbitrage Loop**:

The examples below assume the relevant direction is enabled, the reserve-token quote is usable, ordinary interval and sender-allowlist checks pass, and fees and execution costs are zero. The redemption example also assumes sufficient reserve liquidity.

```
GREEN trading at $0.97?
1. Buy 10,000 GREEN on DEX for $9,700
2. Redeem through the PSM for the configured reserve stablecoin
3. Illustrative gross spread: $300 before fees and execution costs
4. Your buying pressure can help restore the peg

GREEN trading at $1.03?
1. Mint 10,000 GREEN through the PSM with the configured reserve stablecoin
2. Sell on DEX for $10,300
3. Illustrative gross spread: $300 before fees and execution costs
4. Your selling pressure can help restore the peg
```

**Why It Works**:

* **No zone prerequisite** — no position in a redemption or liquidation zone is required; both directions still depend on enablement, usable reserve-token pricing, available input and capacity, a valid recipient, and any ordinary sender allowlist, while redemption additionally requires reserve liquidity
* **Bounded quotes** — conversion uses the applicable oracle/par rule and configured fees
* **Single transaction** — conversion completes in one submitted transaction when its checks pass
* **Optional reserve yield** — configured reserves can use a compatible yield position while waiting

**The Fine Print**:

Interval limits and optional sender allowlists govern ordinary calls. Recipient classification is separate: a recipient recognized as an Underscore vault bypasses the ordinary allowlist, fee, and interval controls. Minting remains bounded by the submitted reserve input and its conservative quote, while redemption remains bounded by available reserves.

Bottom line: when configured and available, the PSM supplies another arbitrage path that can help close a GREEN price gap.

## Additional GREEN Ecosystem Features

### Bad Debt Resolution

If recorded bad debt exists during a [bond](../governance-and-economics/03-bonds.md) purchase, the oracle-valued payment can clear recorded bad debt up to that value. The corresponding RIPE payout is accounted outside ordinary bond-distribution usage, but authorized cap administration must count it separately toward RIPE's protocol-wide 1 billion-token cap across all blockchains.

### Protocol-Wide Integration

* GREEN is the only borrowable asset in Ripe Protocol
* Liquidation debt is credited in GREEN-value units: auctions pay GREEN, while configured Stability routes may consume GREEN, redeem sGREEN to GREEN, or transfer eligible non-GREEN settlement assets
* Deleverage is a separate debt-reduction route with its own configured asset handling
* Fee flows strengthen the ecosystem
* Designed for maximum composability with DeFi

### Keeper Network Protection

* Permissionless keepers can monitor positions and submit eligible liquidation calls; monitoring coverage and timing are not guaranteed
* Trigger liquidations when positions become unsafe
* Productive keeper calls can earn a configured, bounded reward minted as GREEN directly or wrapped into sGREEN
* Minimizes bad debt through rapid liquidation execution

## GREEN Throughout Ripe Protocol

### Transform to sGREEN for Revenue-Backed Yield

[sGREEN](../earning-and-rewards/01-sgreen.md) is GREEN's yield-bearing twin:

* **Set and Forget**: Deposit GREEN, receive sGREEN
* **Share-Based Growth**: Configured revenue can increase GREEN backing per share without rebasing
* **Protocol Revenue**: Receives the configured share of daowry and borrowing-triggered flushed accrued interest
* **Conditional Liquidity**: Redeem for GREEN subject to available backing and token or vault controls
* **Configured Use**: Stability pools can accept sGREEN when supported

### Participate in Stability Pools

Deploy sGREEN or GREEN LP tokens in [stability pools](../earning-and-rewards/02-stability-pools.md) for potential liquidation proceeds:

* **Three Potential Return Sources**: sGREEN backing growth, liquidation proceeds, and configured RIPE rewards
* **Configured Spread**: Receive collateral through the configured liquidation spread
* **Support Protocol**: Your deposits enable smooth liquidations
* **Conditional Withdrawal**: Exits depend on unreserved custody and ordinary vault controls

### Enable Borrowing Options

When taking loans, receive GREEN flexibly:

* **Direct GREEN**: Standard stablecoin for any use
* **Auto-sGREEN**: Receive sGREEN at its current exchange rate; future backing growth depends on configured revenue
* **Stability Pool Entry**: Combine potential sGREEN backing growth, liquidation proceeds, and configured RIPE rewards

### Earn RIPE Block Rewards

Multiple ways to earn [RIPE rewards](../earning-and-rewards/03-ripe-rewards.md) through GREEN:

* **Borrowing GREEN**: Larger, longer-term loans earn more rewards
* **Stability Pool Deposits**: Eligible configured Stability positions can earn RIPE
* **Time-Weighted System**: Points accumulate based on position size × time
* **Potential Cost Offset**: Configured RIPE rewards can reduce effective borrowing costs or add to Stability-pool returns

## GREEN vs The Competition: A Clear Winner

When you need to borrow stablecoins, GREEN offers fundamental advantages:

**vs Centralized Stables (USDC/USDT)**:

* Can't borrow USDC/USDT — you must buy them
* GREEN: Mint by borrowing against supported collateral with configured borrowing power
* GREEN can be deposited into sGREEN for potential backing growth

**vs Traditional Crypto Stables (DAI/LUSD)**:

* DAI: Separate vaults for each collateral type
* LUSD: Only accepts ETH (or similar) as collateral
* GREEN: One loan backed by eligible collateral across your portfolio

**vs Algorithmic Experiments (UST)**:

* No real backing = inevitable collapse
* GREEN: Borrowing is limited by configured collateral and debt terms

**The Key Difference**: GREEN is built for borrowers who want to use eligible collateral across their portfolio efficiently. Supported tokenized stocks can work alongside stablecoins, crypto assets, yield positions, and other eligible collateral to back a single, manageable loan position.

## Why This Matters

Every other stablecoin makes you choose: safety or efficiency, yield or liquidity, simplicity or power.

GREEN combines one position backed by eligible collateral across your portfolio with multiple optional mechanisms. Configured revenue can increase sGREEN backing, Stability pools can receive liquidation collateral, and conditional redemption, PSM, interest-rate, and treasury routes can support the peg. None guarantees yield or a fixed market price.

Stop settling for stablecoins designed for 2020. This is how money works in DeFi now.

***

_For technical implementation details, see the_ [_GreenToken Technical Documentation_](https://ripe-finance.gitbook.io/ripe-developers/tokens/greentoken)_._
