---
description: How Ripe Selects and Validates Asset Prices
---

# Price Oracles: How Ripe Selects Asset Prices

Ripe routes asset valuation through a Price Desk and registered source adapters. The Price Desk checks configured priority sources first, then other registered sources, and returns the first usable nonzero price. A failure in one source is isolated so later sources can still answer.

This is ordered failover, not consensus: the Price Desk does not medianize, average, or require agreement among sources before accepting the first usable result.

If no source can establish a usable price, the protocol fails closed rather than inventing a value or relying on a last-known cache.

## Why Pricing Matters in Ripe

Every critical protocol operation depends on accurate pricing:

* **Borrowing Power**: Your collateral value determines how much GREEN you can borrow
* **Liquidation Safety**: Price movements trigger liquidations when positions become risky
* **Redemption Values**: Credit redemption treats GREEN as a $1 debt unit and determines the collateral amount from a usable oracle price
* **Stability Settlement**: Collateral and settlement values depend on usable prices
* **Interest Rates**: Dynamic rates can use confirmed observations from a configured GREEN reference pool

## Price Source Routing

Instead of hard-coding one price feed, Ripe routes valuation through a registry of source adapters:

```
Asset Price Request Flow:

Your Asset (e.g., WETH)
    ↓
Price Desk (Router)
    ↓
1. Check configured priority sources
2. Isolate an unsupported, stale, reverted, or malformed response
3. If needed, check remaining registered sources
4. Return the first usable nonzero price
5. If none is usable, return unavailable or fail closed for a strict caller
    ↓
Usable USD Value or Explicit Unavailability
```

This design provides:

* **Failure Isolation**: One source revert or malformed response does not abort the entire search
* **Asset Flexibility**: Different source adapters can support different assets
* **Explicit Failure**: No usable source means no price, not a guessed or cached value
* **Extensibility**: Governance can register additional compatible source adapters

## Price Source Adapters

The protocol supports multiple kinds of source adapter. Which adapters are registered, their priority, and their asset coverage are deployment configuration — the mechanism does not assume that every adapter below is active on every chain.

### Chainlink

The Chainlink adapter reads configured aggregator feeds, normalizes their decimals, and rejects values that do not satisfy the adapter's answer and freshness checks. It can serve as a prioritized source for any asset with a configured feed.

### Curve Pools

The Curve adapter derives supported values from configured Curve pool state. It also maintains confirmed observations for a configured GREEN reference pool.

For [dynamic interest rates](02-borrowing.md#dynamic-interest-rates), each qualifying interval uses the lower ratio from two consecutive reference-pool observations. Qualifying intervals are weighted by duration, and excessive gaps or stale history are excluded. This makes the rate input depend on sustained recorded conditions rather than one isolated observation.

### Pyth Network

The Pyth adapter can support configured feeds whose publisher data is supplied through an update transaction. Its own feed identity, value, confidence, and timestamp checks determine whether a response is usable.

### Stork Network

The Stork adapter reads configured Stork feeds and applies its feed and freshness validation before returning a value. It can provide additional asset coverage when registered and configured.

### Blue Chip Yield Prices

The Blue Chip adapter uses protocol-specific accounting rather than one universal yield-token formula:

* **Aave V3 and Compound V3**: Return the current usable price of the configured underlying asset directly; these branches do not use share-price snapshots
* **Morpho, Morpho V2, Euler, and Fluid ERC-4626 vaults**: Combine the current usable underlying price with a conservative share rate derived from weighted snapshots and the current live conversion rate
* **Moonwell**: Uses its stored exchange-rate accounting with the same weighted-snapshot and current-rate comparison

For snapshot-based branches, new observations can throttle configured upside changes, while a lower current conversion rate is accepted immediately. For example, to price a hypothetical Morpho USDC position:

* **Underlying USDC price**: Assumed at $1.00 from the first usable configured source
* **Morpho share price**: Weighted average over time (e.g., 1.05 USDC per share)
* **Final position value**: $1.00 × 1.05 = $1.05 per share

Only the share or exchange rate uses this snapshot treatment; the underlying asset price remains current. If either required component is unavailable or invalid, that branch returns no usable price.

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
* New liquidation, redemption, and deleveraging passes are withheld until valuation recovers; an already-created auction remains subject to its own price and settlement checks
* Standard GREEN repayment remains the dependable public recovery path, subject to its normal controls

Ordinary Teller deposits and withdrawals run strict whole-account debt housekeeping and can revert while a debt-bearing price remains unusable. Adding collateral is therefore not a guaranteed quarantine-recovery path.

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

A source adapter's pause flag freezes guarded feed-configuration and update or snapshot operations. It does **not** disable that adapter's existing price reads, so a paused source can still answer the Price Desk from already configured state. Disabling a feed or changing routing priority is a separate action.

### Fail-Safe Mechanisms

* First-usable-source failover across registered adapters
* Isolated source calls so one failure does not mask a later healthy source
* Strict callers fail closed when coverage exists but no source establishes a usable value
* Source pause controls for guarded configuration, update, and snapshot operations without implicitly changing read routing
* Fund recovery for stuck update fees
* Governance-controlled source and priority configuration

## Selection and Failure Semantics

The pricing rule is deterministic: check configured priority sources first, continue through other registered sources when an earlier one is unusable, and accept the first usable nonzero value. Multiple sources provide ordered fallback; they do not corroborate or combine their answers.

Redundancy improves availability, but it is not permission to guess. If every applicable source is stale, unavailable, or failing—or recorded vault backing is unusable—Ripe exposes that absence and protects debt-bearing accounts through quarantine. Positions resume normal valuation when the relevant price or backing failure has cleared and every remaining debt-bearing balance is usable.
