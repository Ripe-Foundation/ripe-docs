---
description: The Truth About Your Money
---

# Price Oracles: The Truth About Your Money

One bad price feed can destroy a protocol. Positions liquidated on fake spikes. Exploits draining millions. Users losing everything to a malicious update.

Ripe routes across registered price-source adapters in configured priority order and returns the first usable price. If an earlier source reverts, returns malformed data, or cannot provide a usable price, the Price Desk isolates that result and continues to later sources. An asset's available sources depend on deployment configuration, so a particular asset may have only one usable source.

Which adapters are registered, their priority, and their asset coverage vary by deployment and governance configuration. See [RIPE Params](https://params.ripe.finance) for current onchain settings.

Your collateral value comes from the first usable source in the configured order.

For ERC-20 valuation, a usable price feed is only one prerequisite: Price Desk must also have the token's scale synchronized. Without it, non-strict conversions report no value and strict conversions revert.

## Why Pricing Matters in Ripe

Every critical protocol operation depends on accurate pricing:

* **Borrowing Power**: Your collateral value determines how much GREEN you can borrow
* **Liquidation Safety**: Price movements trigger liquidations when positions become risky
* **Redemption Values**: Direct redemptions treat GREEN as a $1 debt-value input while oracle pricing and actual delivery determine collateral and debt credit
* **Stability Settlement**: Configured liquidation spreads are calculated from usable oracle prices
* **Interest Rates**: Dynamic rates respond to GREEN's market price

With so much at stake, we've built a pricing system that's both robust and transparent.

## Ordered Oracle Routing

When multiple adapters cover an asset, Ripe routes across them in configured order:

```
Asset Price Request Flow:

Your Asset (e.g., a configured tokenized stock or ETH)
    ↓
Price Desk (Ordered Router)
    ↓
1. Check configured priority source IDs
2. If no usable price, check the remaining registered source IDs
3. Return the first usable nonzero price
    ↓
First Usable USD Value
```

This design provides several benefits:

* **Failure Isolation**: A failed source does not prevent a later configured usable source from answering
* **Asset Flexibility**: Different oracles support different assets
* **Cost Optimization**: Use expensive oracles only for critical assets
* **Future-Proof**: New oracle providers can be added seamlessly

## Our Oracle Providers

The adapters below describe mechanisms the protocol can support; the live set is deployment configuration rather than a fixed inventory in these docs.

### 1. Chainlink

The industry standard for decentralized price feeds:

* **Coverage**: Major crypto assets and equity-reference feeds when configured
* **Reliability**: Battle-tested across DeFi with billions secured
* **Update Frequency**: Varies by asset based on volatility
* **Trust Model**: Decentralized node operators with reputation

The Chainlink adapter can provide prices for configured mainstream assets. Its live role, asset coverage, and priority vary by deployment.

### 2. Curve Pools (Specialized)

Direct pricing from the largest stablecoin liquidity pools:

* **Coverage**: Stablecoins, Curve LP tokens, GREEN pairs
* **Reliability**: Based on actual tradeable liquidity
* **Special Feature**: Monitors GREEN's peg in real-time
* **Trust Model**: On-chain AMM state with adapter-specific validation and pool-liquidity risk

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

### Stock-Market Hours and Price Gaps

Ripe does not read an exchange calendar or switch into a separate mode when a stock reference market closes. Price Desk continues to use its ordinary ordered routing and freshness checks, and a tokenized stock's last published price may remain usable while the configured source still considers it fresh.

Robinhood's [oracle guidance](https://docs.robinhood.com/chain/oracles-and-price-feeds/) exposes an advisory `oraclePaused()` flag for corporate-action windows and recommends treating it as temporary price unavailability. Ripe's standard Chainlink adapter does not read that token flag. During pricing it calls `latestRoundData()` and normalizes the result using feed decimals verified and stored when the feed configuration is confirmed, then applies its ordinary answer, round, timestamp, and freshness checks. A still-valid, still-fresh round can therefore remain usable while `oraclePaused()` is true. Once that round is no longer usable, Price Desk tries the next configured source.

If every configured source becomes unusable, Ripe does not substitute an indefinitely cached price.

When a usable source resumes, the next accepted price can revalue tokenized-stock collateral in one step. A reference-market reopening gap can therefore change account health abruptly, and subsequent borrowing, withdrawal, redemption, deleverage, and liquidation checks use the recovered price.

For a Stock Token that uses `uiMultiplier()`, the configured feed must return the token-level USD price with that corporate-action adjustment already incorporated. Ripe consumes that price once and does not independently read or apply `uiMultiplier()`. See [Stock Tokens as Collateral](03-collateral-assets.md#stock-tokens-as-collateral).

### What Happens When Prices Go Stale?

1. **Earlier Source Stale**: The query continues to the next configured source
2. **All Sources Unusable**: A non-strict query reports no price and a strict valuation fails closed; the protocol does not substitute a last-known cached value
3. **No Feed Available**: New assets without feeds cannot be borrowed against

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
* Governance-controlled feed and adapter disable or replacement paths
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
