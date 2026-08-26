---
description: The Truth About Your Money
---

# Price Oracles: The Truth About Your Money

One bad price feed can destroy a protocol. Positions liquidated on fake spikes. Exploits draining millions. Users losing everything to a malicious update.

Ripe doesn't rely on any single source. We check configured source adapters in priority order — taking the first usable price. If one source reverts, returns malformed data, or cannot provide a usable price, the Price Desk isolates that result and continues to later sources.

Which adapters are registered, their priority, and their asset coverage vary by deployment and governance configuration. See [RIPE Params](https://params.ripe.finance) for current onchain settings.

Your collateral value comes from the first usable source in the configured order.

For ERC-20 valuation, a usable price feed is only one prerequisite: Price Desk must also have the token's scale synchronized. Without it, non-strict conversions report no value and strict conversions revert.

## Why Pricing Matters in Ripe

Every critical protocol operation depends on accurate pricing:

* **Borrowing Power**: Your collateral value determines how much GREEN you can borrow
* **Liquidation Safety**: Price movements trigger liquidations when positions become risky
* **Redemption Values**: Direct redemptions treat GREEN as a $1 debt-value input while oracle pricing and actual delivery determine collateral and debt credit
* **Stability Pool Profits**: Liquidation discounts are calculated from current market prices
* **Interest Rates**: Dynamic rates respond to GREEN's market price

With so much at stake, we've built a pricing system that's both robust and transparent.

## The Multi-Oracle Advantage

Instead of relying on a single price feed, Ripe routes across configured price-source adapters:

```
Asset Price Request Flow:

Your Asset (e.g., ETH)
    ↓
Price Desk (Ordered Router)
    ↓
1. Check configured priority source IDs
2. If no usable price, check the remaining registered source IDs
3. Return the first usable nonzero price
    ↓
Accurate USD Value
```

This design provides several benefits:

* **Failure Isolation**: A failed source does not prevent a later configured usable source from answering
* **Asset Flexibility**: Different oracles support different assets
* **Cost Optimization**: Use expensive oracles only for critical assets
* **Future-Proof**: New oracle providers can be added seamlessly

## Our Oracle Providers

The adapters below describe mechanisms the protocol can support; the live set is deployment configuration rather than a fixed inventory in these docs.

### 1. Chainlink (Primary)

The industry standard for decentralized price feeds:

* **Coverage**: Major crypto assets (ETH, BTC, stablecoins, blue chips)
* **Reliability**: Battle-tested across DeFi with billions secured
* **Update Frequency**: Varies by asset based on volatility
* **Trust Model**: Decentralized node operators with reputation

Chainlink serves as our primary oracle for most mainstream assets due to its proven track record and wide deployment.

### 2. Curve Pools (Specialized)

Direct pricing from the largest stablecoin liquidity pools:

* **Coverage**: Stablecoins, Curve LP tokens, GREEN pairs
* **Reliability**: Based on actual tradeable liquidity
* **Special Feature**: Monitors GREEN's peg in real-time
* **Trust Model**: On-chain AMM state, manipulation-resistant

**Critical for GREEN Stability**: The Curve price feed maintains the configured Green Reference Pool data that directly impacts [dynamic interest rates](02-borrowing.md#dynamic-interest-rates). Each qualifying interval uses the lower GREEN ratio from two consecutive observations, weights that interval by duration, and excludes excessive gaps or stale history. This makes the rate input depend on sustained recorded conditions rather than one isolated observation.

### 3. Pyth Network (High-Frequency)

Sub-second price updates from institutional providers:

* **Coverage**: Wide range including stocks, forex, commodities, and crypto
* **Reliability**: Professional market makers provide data
* **Update Frequency**: Can update every slot if needed
* **Trust Model**: Aggregated from multiple institutional sources

Useful for assets requiring frequent updates or those not covered by Chainlink.

### 4. Stork Network (Emerging)

Next-generation oracle with unique features:

* **Coverage**: Growing selection of DeFi assets
* **Reliability**: Novel cryptographic attestation model
* **Update Frequency**: On-demand with nanosecond precision
* **Trust Model**: Decentralized publishers with stakes

Provides additional redundancy and supports newer assets.

### 5. Blue Chip Yield Prices (Specialized)

Custom pricing for yield-bearing tokens from major protocols:

* **Coverage**: Aave V3 and Compound V3 positions, Morpho and Morpho V2 positions, Euler, Fluid, Moonwell, etc.
* **Reliability**: Direct integration with protocol contracts
* **Special Feature**: Handles rebasing and yield accrual correctly
* **Trust Model**: Based on underlying protocol's accounting

Blue Chip Yield Prices supports several adapter models. Aave V3 and Compound V3 positions use the underlying asset price directly and do not use share-rate snapshots. Morpho, Morpho V2, Euler, and Fluid use duration-weighted ERC-4626 share-rate snapshots combined with the underlying price. Moonwell uses a separate exchange-rate route.

For example, a Morpho USDC position uses two-layer pricing:

* **Underlying USDC price**: Fetched in real-time from Chainlink ($1.00)
* **Morpho share price**: Weighted average over time (e.g., 1.05 USDC per share)
* **Final position value**: $1.00 × 1.05 = $1.05 per share

**Manipulation Protection**: Where share or exchange-rate snapshots apply, only that rate uses weighted snapshots — the underlying asset price remains current. This reduces exposure to temporary share-rate spikes while allowing the collateral to respond to movements in the underlying asset. Applicable snapshots also include upside-deviation throttling, gradually incorporating sudden appreciation rather than accepting it immediately.

## Staleness Protection

Stale prices are dangerous. Here's how we prevent them:

### Global Staleness Threshold

* Freshness windows are set through deployment and governance configuration
* For adapters that consume the router-supplied window, a feed-specific nonzero window can override the global policy; snapshot adapters may instead use local freshness configuration
* See [RIPE Params](https://params.ripe.finance) for current onchain settings

### Per-Oracle Configuration

Each oracle can have custom staleness limits:

* Chainlink: Uses round timestamp
* Pyth/Stork: Uses publish timestamp
* Curve: Reads pool state directly; confirmed GREEN reference history has its own block-based freshness rules
* Blue Chip: Adapter-specific; share-rate snapshot routes use local delay and freshness settings, while direct-underlying routes do not use those snapshots

Freshness enforcement is adapter-specific. Blue Chip snapshot routes and Undy vault pricing use their locally configured snapshot freshness rather than the forwarded global stale-time setting.

### What Happens When Prices Go Stale?

1. **Earlier Source Stale**: The query continues to the next configured source
2. **All Sources Unusable**: A non-strict query reports no price and a strict valuation fails closed; the protocol does not substitute a last-known cached value
3. **No Feed Available**: New assets without feeds cannot be borrowed against

For an account with outstanding debt, a positive balance of borrowing-power collateral with no usable price — or a recorded balance with no usable vault backing — creates a valuation quarantine:

* The unavailable collateral contributes no borrowing power
* New borrowing and withdrawals of collateral that supports the debt are blocked
* New liquidation, redemption, and deleveraging passes are withheld until valuation recovers; an already-created auction remains subject to its own price and settlement checks
* Standard GREEN repayment remains the dependable public recovery path, subject to its normal controls

Ordinary Teller deposits and withdrawals run strict whole-account debt housekeeping and can revert while a debt-bearing price remains unusable. Adding collateral is therefore not a guaranteed quarantine-recovery path.

Quarantine is not itself a liquidation trigger or a declaration of insolvency. Once pricing or backing becomes usable again, the account returns to the normal health checks using the recovered value.

## Price Priority System

Not all oracles are created equal. The priority system selects the first usable source in the configured order:

```
Priority Order (Configurable):
1. Check configured priority source IDs
2. Check the remaining registered source IDs
3. Return the first usable nonzero price
```

Governance can adjust priorities based on:

* Oracle reliability track record
* Gas costs for updates
* Asset-specific considerations
* Market conditions

## Security Measures

### Time-Locked Changes

Oracle additions, priority changes, feed updates, and emergency controls follow their configured governance and permission paths. Exact delays and permissions vary by deployment; see [RIPE Params](https://params.ripe.finance) for current onchain settings.

### Fail-Safe Mechanisms

* Ordered fallback across configured sources
* Isolated source calls so one failure does not mask a later healthy source
* Strict valuation fails closed when no source establishes a usable value
* Pause functionality for compromised feeds
* Fund recovery for stuck update fees
* Governance override capabilities

## Trust Through Verification

Here's what your lending protocol won't tell you: they probably use one oracle. Maybe two if they're fancy.

Ripe can connect to multiple independent price sources. Price Desk checks them in configured order and continues past an unusable source. If no source returns a usable price, non-strict queries report no price and strict valuation fails closed; fallback improves resilience but does not guarantee uninterrupted pricing.

***

_For technical implementation details, see:_

* [_Price Desk Technical Documentation_](https://ripe-finance.gitbook.io/ripe-developers/pricing/pricedesk) _- Ordered price routing_
* [_Chainlink Prices_](https://ripe-finance.gitbook.io/ripe-developers/pricing/chainlinkprices) _- Chainlink integration_
* [_Curve Prices_](https://ripe-finance.gitbook.io/ripe-developers/pricing/curveprices) _- AMM-based pricing and GREEN monitoring_
* [_Pyth Prices_](https://ripe-finance.gitbook.io/ripe-developers/pricing/pythprices) _- High-frequency oracle updates_
* [_Stork Prices_](https://ripe-finance.gitbook.io/ripe-developers/pricing/storkprices) _- Decentralized price attestations_
* [_Blue Chip Yield Prices_](https://ripe-finance.gitbook.io/ripe-developers/pricing/bluechipyieldprices) _- Yield token valuations_
