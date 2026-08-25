---
description: The Truth About Your Money
---

# Price Oracles: The Truth About Your Money

One bad price feed can destroy a protocol. Positions liquidated on fake spikes. Exploits draining millions. Users losing everything to a malicious update.

Ripe's Price Desk can consult multiple registered price sources. It checks configured priority sources first, then other registered sources, and returns the first usable nonzero price. A failure in one source is isolated so later sources can still answer.

If no source can establish a usable price, the protocol fails closed rather than inventing a value or relying on a last-known cache.

## Why Pricing Matters in Ripe

Every critical protocol operation depends on accurate pricing:

* **Borrowing Power**: Your collateral value determines how much GREEN you can borrow
* **Liquidation Safety**: Price movements trigger liquidations when positions become risky
* **Redemption Values**: Direct redemptions exchange GREEN for exactly $1 of collateral
* **Stability Settlement**: Collateral and settlement values depend on usable prices
* **Interest Rates**: Dynamic rates use sustained, corroborated observations from a configured GREEN reference pool

With so much at stake, we've built a pricing system that's both robust and transparent.

## The Multi-Oracle Advantage

Instead of hard-coding one price feed, Ripe routes valuation through a registry of source adapters:

```
Asset Price Request Flow:

Your Asset (e.g., ETH)
    ↓
Price Desk (Aggregator)
    ↓
1. Check configured priority sources
2. Isolate an unsupported, stale, reverted, or malformed response
3. If needed, check remaining registered sources
4. Return the first usable nonzero price
5. If none is usable, return unavailable or fail closed for a strict caller
    ↓
Usable USD Value or Explicit Unavailability
```

This design provides several benefits:

* **Failure Isolation**: One source revert or malformed response does not abort the entire search
* **Asset Flexibility**: Different source adapters can support different assets
* **Explicit Failure**: No usable source means no price, not a guessed or cached value
* **Future-Proof**: Governance can register additional compatible source adapters

## Price Source Adapters

The protocol supports multiple kinds of source adapter. Which adapters are registered, their priority, and their asset coverage are deployment configuration — the mechanism does not assume that every adapter below is active on every chain.

### Chainlink

The industry standard for decentralized price feeds:

* **Coverage**: Major crypto assets (ETH, BTC, stablecoins, blue chips)
* **Reliability**: Battle-tested across DeFi with billions secured
* **Update Frequency**: Varies by asset based on volatility
* **Trust Model**: Decentralized node operators with reputation

Chainlink can serve as a prioritized source for assets with configured feeds.

### Curve Pools

Direct pricing from the largest stablecoin liquidity pools:

* **Coverage**: Stablecoins, Curve LP tokens, GREEN pairs
* **Reliability**: Based on actual tradeable liquidity
* **Special Feature**: Maintains confirmed reference-pool observations for GREEN
* **Trust Model**: On-chain AMM state, manipulation-resistant

**Critical for GREEN Stability**: The Curve source maintains chronological snapshots of a configured GREEN reference pool for [dynamic interest rates](02-borrowing.md#dynamic-interest-rates). Each interval uses the lower ratio from two consecutive observations, so both endpoints must corroborate an imbalance. Qualifying intervals are weighted by duration, and excessive gaps or stale history are excluded. This makes the rate signal resistant to an isolated observation while still responding to sustained weakness.

### Pyth Network

Sub-second price updates from institutional providers:

* **Coverage**: Wide range including stocks, forex, commodities, and crypto
* **Reliability**: Professional market makers provide data
* **Update Frequency**: Can update every slot if needed
* **Trust Model**: Aggregated from multiple institutional sources

This adapter can support assets requiring publisher-supplied updates when it is configured for a deployment.

### Stork Network

Next-generation oracle with unique features:

* **Coverage**: Growing selection of DeFi assets
* **Reliability**: Novel cryptographic attestation model
* **Update Frequency**: On-demand with nanosecond precision
* **Trust Model**: Decentralized publishers with stakes

This adapter can provide additional asset coverage when it is registered and configured.

### Blue Chip Yield Prices

Custom pricing for yield-bearing tokens from major protocols:

* **Coverage**: aTokens (Aave), cTokens (Compound), Morpho positions, Euler, etc.
* **Reliability**: Direct integration with protocol contracts
* **Special Feature**: Handles rebasing and yield accrual correctly
* **Trust Model**: Based on underlying protocol's accounting

**Two-Layer Pricing**: This oracle combines a current usable underlying-asset price with weighted snapshots of the share price or exchange rate. For example, to price a hypothetical Morpho USDC position:

* **Underlying USDC price**: Assumed at $1.00 from the first usable configured source
* **Morpho share price**: Weighted average over time (e.g., 1.05 USDC per share)
* **Final position value**: $1.00 × 1.05 = $1.05 per share

**Manipulation Protection**: Only the exchange rate uses weighted snapshots — the underlying asset price remains current. This limits the effect of flash-loan or temporary exchange-rate spikes while allowing the collateral value to respond to movements in the underlying asset. The snapshots also include upside-deviation throttling for the exchange rate, gradually incorporating sudden appreciation rather than accepting it immediately.

## Freshness and Failure Protection

Each source adapter validates the data needed for its own pricing method. Depending on the adapter, that can include a global freshness policy, an asset-specific feed rule, publisher timestamps, or confirmed snapshot history.

For each source, the Price Desk distinguishes among:

* A usable nonzero price
* A valid response that has no feed for the asset
* A configured feed that currently has no usable price, including stale data
* A reverted, malformed, or otherwise invalid response

Unsupported assets and isolated source failures do not prevent the Price Desk from checking later sources. A stale or failed primary source can therefore fall through to another usable source without a manual intervention.

### What Happens When No Source Is Usable?

The protocol does **not** substitute a last-known cached price. A non-strict query reports the price as unavailable; a strict valuation fails closed rather than continuing with an unverified value.

For an account with outstanding debt, a positive balance of borrowing-power collateral with no usable price — or a recorded balance with no usable vault backing — creates a valuation quarantine:

* The unavailable collateral contributes no borrowing power
* New borrowing and withdrawals of collateral that supports the debt are blocked
* Liquidation, redemption, and deleveraging are withheld until valuation recovers
* Repayment and collateral-addition recovery paths remain available, subject to normal protocol controls

Quarantine is not itself a liquidation trigger or a declaration of insolvency. Once pricing or backing becomes usable again, the account returns to the normal health checks using the recovered value.

## Price Priority System

Not all sources have the same trust model or asset coverage. The Price Desk therefore uses a configurable search order:

```
Configured priority source IDs
    ↓
Remaining registered source IDs not already checked
    ↓
First usable nonzero price, or explicit unavailability
```

Governance can adjust priorities based on:

* Source reliability and trust model
* Asset coverage
* Operational cost
* Deployment-specific risk policy

## Security Measures

### Controlled Configuration

Source registration, priority, feed configuration, and emergency controls follow their governance and permission paths. Their exact timelocks and parameters are deployment configuration rather than properties of the aggregation algorithm.

### Fail-Safe Mechanisms

* First-usable-source failover across registered adapters
* Isolated source calls so one failure does not mask a later healthy source
* Strict callers fail closed when coverage exists but no source establishes a usable value
* Pause functionality for compromised feeds
* Fund recovery for stuck update fees
* Governance-controlled source and priority configuration

## Trust Through Verification

Ripe's pricing rule is simple to verify: check configured priority sources first, continue through other registered sources when an earlier one is unusable, and accept only the first usable nonzero value.

Redundancy improves availability, but it is not permission to guess. If every applicable source is stale, unavailable, or failing, Ripe exposes that absence and protects debt-bearing accounts through quarantine. Positions resume normal valuation only when a source can establish a usable price again.

***

_For technical implementation details, see:_

* [_Price Desk Technical Documentation_](https://ripe-finance.gitbook.io/ripe-developers/pricing/pricedesk) _- Oracle aggregation system_
* [_Chainlink Prices_](https://ripe-finance.gitbook.io/ripe-developers/pricing/chainlinkprices) _- Chainlink integration_
* [_Curve Prices_](https://ripe-finance.gitbook.io/ripe-developers/pricing/curveprices) _- AMM-based pricing and GREEN monitoring_
* [_Pyth Prices_](https://ripe-finance.gitbook.io/ripe-developers/pricing/pythprices) _- High-frequency oracle updates_
* [_Stork Prices_](https://ripe-finance.gitbook.io/ripe-developers/pricing/storkprices) _- Decentralized price attestations_
* [_Blue Chip Yield Prices_](https://ripe-finance.gitbook.io/ripe-developers/pricing/bluechipyieldprices) _- Yield token valuations_
