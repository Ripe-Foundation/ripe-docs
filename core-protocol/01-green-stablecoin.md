---
description: The Stablecoin That Works Harder.
---

# GREEN: The Stablecoin That Works Harder

Your USDC sits there doing nothing. Your DAI requires a new vault for every asset. Your LUSD only accepts ETH.

GREEN? It's built different. Mint it against eligible borrowing collateral across your portfolio — ETH, stablecoins, NFTs, and other supported assets. Earn yield automatically through [sGREEN](../earning-and-rewards/01-sgreen.md). Score liquidation profits in [stability pools](../earning-and-rewards/02-stability-pools.md). Watch six different mechanisms defend the peg while you sleep.

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

Ripe:         ETH + WBTC + USDC + stETH + NFTs + Eligible Assets
              ↓ (eligible collateral combined)
              One GREEN Loan at Weighted Terms
```

Eligible non-Stability collateral across your portfolio — from blue-chip crypto assets to yield-bearing positions, from stablecoins to tokenized stocks — backs a single GREEN loan. This creates unmatched capital efficiency while keeping your risk isolated from other users.

### Complementary, Not Competitive

While these other lending protocols have borrowing limitations, they excel at generating yield — and Ripe turns their yield-bearing tokens into powerful collateral. Instead of choosing between earning yield OR borrowing, you can do both:

* **Aave aTokens**: Earn lending yield while using as Ripe collateral
* **Compound cTokens**: Your supplied assets keep compounding
* **Morpho positions**: Optimized rates become productive collateral
* **Maker sDAI**: Savings rate continues while backing GREEN loans

The best strategy? Lend on these protocols for yield, then use those yield-bearing positions as collateral on Ripe. You get their yields AND our capital efficiency — truly the best of both worlds.

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

### Creation Through Borrowing

Every GREEN token represents real value locked in Ripe Protocol:

1. **Deposit Collateral**: Lock any [supported asset](03-collateral-assets.md) (ETH, WBTC, stablecoins, etc.)
2. **Borrow GREEN**: Mint new GREEN against your collateral
3. **Configured Collateralization**: Asset-specific LTV terms determine borrowing power
4. **Pay Interest**: [Dynamic rates](02-borrowing.md#dynamic-interest-rates) that respond to market conditions
5. **Origination Fee (Daowry)**: A configured one-time fee flows through protocol revenue allocation

**Hypothetical Multi-Asset Borrowing Example**:

```
Your Unified Portfolio:
- 5 ETH worth $10,000 (70% LTV = $7,000 borrowing power)
- 10,000 USDC (80% LTV = $8,000 borrowing power)
- 2 cbETH worth $4,000 (70% LTV = $2,800 borrowing power)
- 1M DEGEN worth $1,000 (40% LTV = $400 borrowing power)
- 1 Bored Ape worth $50,000 (40% LTV = $20,000 borrowing power)

Total Borrowing Power: $38,200 GREEN
Single Loan, Single Interest Rate, All Assets Working Together
```

The assets and values above are illustrative. See [RIPE Params](https://params.ripe.finance) for current asset support and configuration.

### Destruction Through Repayment

GREEN supply contracts automatically when loans are repaid:

1. **Send GREEN**: Return borrowed amount plus interest
2. **Burn Forever**: GREEN is permanently destroyed
3. **Unlock Collateral**: Get your assets back proportionally

This elegant mechanism ensures GREEN supply expands and contracts with real borrowing demand.

Standard repayment returns any payment above the live debt to the payer. A full payoff does not need to traverse or reprice the borrower's collateral, so repayment remains the recovery path when a debt-bearing asset is quarantined by unavailable pricing or backing. If a partial repayment cannot derive eligible replacement terms, the stored debt terms are preserved.

### Cross-Chain Token Movement

Configured CCIP pools move GREEN or RIPE with a burn-and-mint design: the token-specific source pool burns the amount sent, and the corresponding token-specific destination pool mints to the recipient. Each deployment must separately authorize its pool to mint, and route availability and limits remain deployment configuration.

## The Six Pillars of Stability

GREEN uses multiple interconnected stability mechanisms with different triggers and authorization paths. Its redemption and conversion mechanisms can create arbitrage loops that support the peg from multiple angles:

### 1. Overcollateralization Foundation

The bedrock of GREEN's stability:

* **Configured Ratios**: Collateralization depends on each asset's governed debt terms
* **Extensible Asset Support**: Different collateral types can be added when compatible custody, pricing, and risk configuration exist
* **Portfolio Effect**: When one asset drops, others may rise — true diversification
* **Real-Time Monitoring**: Continuous health checks on all positions using [price oracles](06-price-oracles.md)
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
* Repayment creates buying pressure, restoring peg

**Above Peg Response (GREEN Strong)**:

* Normal base rates apply when the reference-pool signal is below the configured trigger
* Endaoment can mint GREEN to add liquidity
* Increased supply brings price back down

**Confirmed Recovery**: Mixed safe/danger intervals do not erase accumulated danger duration. Consecutive safe endpoints must cover the configured freshness window before that history resets.

### 3. Direct Redemption Mechanism

Redemptions create an automatic arbitrage loop that restores GREEN's peg whenever it trades below $1. For a detailed explanation of how redemptions work as a protective buffer, see [The Redemption Buffer](04-liquidations.md#the-redemption-buffer) in the liquidations documentation.

**$1 Accounting and Rounding**:

* GREEN is treated as a $1 debt-value input when sizing a redemption; the inverse asset quote rounds down, and debt credit follows the amount actually credited by the vault
* Targets only positions in the "Redemption Zone" (at or below the redemption threshold)
* No committees, no voting, no delays — instant execution
* Creates a hard floor for GREEN's market price

**How Arbitrage Restores the Peg**:

```
GREEN trading at $0.97? Here's what happens:

1. Arbitrageurs spot the 3% discount
2. Buy 10,000 GREEN for $9,700 on the market
3. Submit a redemption sized for up to $10,000 worth of eligible collateral
4. Profit: $300 risk-free
5. This buying pressure pushes GREEN back to $1
```

**Important Constraint**: Redemptions are only possible when positions exist in the Redemption Zone. During stable markets with healthy collateral ratios, direct redemptions may be unavailable. The protocol also withholds redemption while any debt-bearing collateral in the account has unavailable pricing or unusable backing.

This mechanism activates when market stress pushes positions toward liquidation, providing buying pressure precisely when GREEN needs support most.

### 4. Stability Pool Redemption Mechanism

A second powerful redemption path exists through [stability pools](../earning-and-rewards/02-stability-pools.md) that hold liquidated collateral:

**How Pool Redemptions Work**:

* Stability deposits are represented by shares whose NAV includes unreserved settlement assets and strictly priced active claim assets
* Small liquidation claims can remain dormant until they qualify for activation; active claims can later retire under bounded residual rules
* Public claims and redemptions use batch routes and require usable pricing, available claim custody, and unreserved settlement liquidity
* A compatible liquidation can fall back to an ordinary auction when Stability settlement is unavailable
* Creates arbitrage opportunities during periods of liquidation activity

**Important Constraint**: Pool redemptions require the stability pools to hold collateral from recent liquidations. During calm markets with no liquidations, this redemption path may be unavailable.

**Complementary Redemption Paths**:

```
GREEN at $0.96? Potential redemption options:
1. Direct redemption (if positions in Redemption Zone)
2. Pool redemption (if liquidated collateral available)

Both paths subject to availability
```

**When Pool Redemptions Are Most Effective**:

* **Market Stress**: Liquidations increase, filling pools with collateral
* **Price Volatility**: More positions fail, creating redemption opportunities
* **Cascading Events**: Each liquidation enables more GREEN redemptions
* **Self-Balancing**: Redemptions occur precisely when GREEN needs support

### 5. Endaoment Treasury Operations

The [Endaoment](07-endaoment.md) serves as GREEN's financial fortress — a protocol-owned treasury with vast capabilities to defend the peg:

**Capital Arsenal from Bond Sales**:

* Bond proceeds provide stablecoin reserves for immediate deployment
* Treasury assets can be deployed through authorized DeFi operations
* Growing war chest ensures firepower during any market condition
* No reliance on external capital or emergency fundraising

**Switchboard-Authorized Stabilizer Action**:

A Switchboard-authorized caller submits each stabilizer action; the Endaoment does not run on an autonomous scheduler.

```
Normalized pool-balance adjustment:

Normalized GREEN balance exceeds the paired asset?
→ Remove excess GREEN liquidity
→ Burn GREEN tokens permanently
→ Restore pool balance to support price

Normalized paired-asset balance exceeds GREEN?
→ Mint new GREEN (tracked as debt)
→ Add liquidity to deepen markets
→ Restore pool balance to stabilize price
```

**Liquidity Management Powers**:

* **Multi-DEX Operations**: Deploy liquidity across Curve, Uniswap, Aerodrome simultaneously
* **Concentrated Positions**: Use Uniswap V3 or Aero Slipstream for capital-efficient price support
* **Partner Programs**: Mint GREEN paired with partner assets for deeper markets
* **Transaction-Based Rebalancing**: Move capital between pools through authorized transactions

**Strategic Market Making**:

* Can provide buy-side support when GREEN weak
* Can provide sell-side liquidity when GREEN strong
* Profits from spreads flow back to treasury
* Self-funded operations through yield generation

**Why This Matters**:

* **Deep Resources**: An authorized stabilizer action can mint GREEN within the configured pool debt ceiling
* **Multiple Fronts**: Operates across all major DEXes simultaneously
* **Authorized Execution**: Each operation requires a Switchboard-authorized transaction and must pass configured controls
* **Self-Strengthening**: Every intervention generates fees that grow the treasury

The Endaoment transforms from passive treasury to active market participant, with authorized operations that can support GREEN's peg.

### 6. Peg Stability Module (PSM)

Forget complex arbitrage strategies. When a direction is enabled, the PSM converts between GREEN and its reserve stablecoin around a $1 reference. The reserve token is immutable for that PSM deployment and must use six decimals.

**Dead Simple**:

* **Mint GREEN**: Deposit the reserve stablecoin, receive GREEN around the 1:1 reference (minus a fee if applicable)
* **Redeem GREEN**: Burn GREEN, receive the reserve stablecoin around the 1:1 reference (minus a fee if applicable)
* **Reserve Backed**: Redemption is separate from danger-zone collateral and Stability claims, but still depends on enabled configuration and available reserves

**Illustrative, Conditional Arbitrage Loop**:

The examples below assume the relevant direction is enabled, reserves and interval capacity are available, and fees and execution costs are zero.

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

* **No prerequisites** — don't need positions in danger zones or liquidated collateral
* **Bounded quotes** — conversion uses the applicable oracle/par rule and configured fees
* **Fast** — one transaction, done
* **Optional reserve yield** — configured reserves can use a compatible yield position while waiting

**The Fine Print**:

Rate limits and optional allowlists govern ordinary recipients. A recognized Underscore-vault recipient bypasses the ordinary allowlist, fee, and interval controls, but minting remains bounded by the submitted reserve input and its conservative quote, while redemption remains bounded by available reserves.

Bottom line: when configured and available, the PSM supplies another arbitrage path that can help close a GREEN price gap.

## Additional GREEN Ecosystem Features

### Bad Debt Resolution

If bad debt exists during a [bond](../governance-and-economics/03-bonds.md) purchase, the oracle-valued payment can clear bad debt up to that value. The corresponding RIPE payout is accounted outside ordinary bond-distribution usage, but authorized cap administration must count it separately toward RIPE's protocol-wide 1 billion-token cap across all blockchains.

### Protocol-Wide Integration

* GREEN is the only borrowable asset in Ripe Protocol
* All liquidations settle in GREEN
* Fee flows strengthen the ecosystem
* Designed for maximum composability with DeFi

### Keeper Network Protection

* Automated bots monitor all GREEN loans 24/7
* Trigger liquidations when positions become unsafe
* Productive keeper calls can earn a configured, bounded reward paid in GREEN
* Minimizes bad debt through rapid liquidation execution

## GREEN Throughout Ripe Protocol

### Transform to sGREEN for Automatic Yield

[sGREEN](../earning-and-rewards/01-sgreen.md) is GREEN's yield-bearing twin:

* **Set and Forget**: Deposit GREEN, receive sGREEN
* **Auto-Compounding**: Value grows through exchange rate, not rebasing
* **Protocol Revenue**: Captures fees from borrowing, liquidations, and more
* **Instant Liquidity**: Redeem for GREEN anytime
* **Use Everywhere**: Stability pools accept sGREEN for enhanced returns

### Participate in Stability Pools

Deploy sGREEN or GREEN LP tokens in [stability pools](../earning-and-rewards/02-stability-pools.md) for liquidation profits:

* **Triple Yield**: Base sGREEN rate + liquidation profits + RIPE rewards
* **Configured Spread**: Receive collateral through the configured liquidation spread
* **Support Protocol**: Your deposits enable smooth liquidations
* **Conditional Withdrawal**: Exits depend on unreserved custody and ordinary vault controls

### Enable Borrowing Options

When taking loans, receive GREEN flexibly:

* **Direct GREEN**: Standard stablecoin for any use
* **Auto-sGREEN**: Start earning immediately on borrowed funds
* **Stability Pool Entry**: Maximum yield from day one

### Earn RIPE Block Rewards

Multiple ways to earn [RIPE rewards](../earning-and-rewards/03-ripe-rewards.md) through GREEN:

* **Borrowing GREEN**: Larger, longer-term loans earn more rewards
* **Stability Pool Deposits**: Eligible configured Stability positions can earn RIPE
* **Time-Weighted System**: Points accumulate based on position size × time
* **Offset Costs**: Rewards help reduce effective borrowing rates or boost stability pool returns

## GREEN vs The Competition: A Clear Winner

When you need to borrow stablecoins, GREEN offers fundamental advantages:

**vs Centralized Stables (USDC/USDT)**:

* Can't borrow USDC/USDT — you must buy them
* GREEN: Mint by borrowing against supported collateral with configured borrowing power
* Bonus: GREEN earns yield via sGREEN, USDC sits idle

**vs Traditional Crypto Stables (DAI/LUSD)**:

* DAI: Separate vaults for each collateral type
* LUSD: Only accepts ETH (or similar) as collateral
* GREEN: One loan backed by eligible collateral across your portfolio

**vs Algorithmic Experiments (UST)**:

* No real backing = inevitable collapse
* GREEN: Borrowing is limited by configured collateral and debt terms

**The Key Difference**: GREEN is built for borrowers who want to use eligible collateral across their portfolio efficiently. Supported ETH, stablecoins, NFTs, tokenized stocks, and emerging tokens can work together to back a single, manageable loan position.

## Why This Matters

Every other stablecoin makes you choose: safety or efficiency, yield or liquidity, simplicity or power.

GREEN breaks the tradeoffs. One position backed by eligible collateral across your portfolio. Automatic yield that compounds while you sleep. Liquidation profits when others get rekt. A peg that defends itself through pure economics, not faith.

Stop settling for stablecoins designed for 2020. This is how money works in DeFi now.

***

_For technical implementation details, see the_ [_GreenToken Technical Documentation_](https://ripe-finance.gitbook.io/ripe-developers/tokens/greentoken)_._
