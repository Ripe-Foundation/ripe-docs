---
description: The Truth About Your Money
---

# Price Oracles: The Truth About Your Money

One bad price feed can destroy a protocol. Positions liquidated on fake spikes. Exploits draining millions. Users losing everything to a malicious update.

Ripe checks its price sources in priority order and uses the first good answer. If one fails, the next one gets asked. If nothing can price an asset, Ripe stops rather than guesses.

Not every asset has a backup. A stock token usually has exactly one source — a single Chainlink feed — so when that feed goes quiet, the token has no price until it comes back.

> **Live terms live onchain.** Which price sources are live, their priority, each feed's freshness window, and which assets they cover vary by deployment and change over time. Every number on this page is an example. [Params](https://params.ripe.finance) is the source of truth.

## Why Pricing Matters in Ripe

Every critical protocol operation depends on accurate pricing:

* **Borrowing Power**: Your collateral value determines how much GREEN you can borrow
* **Liquidation Safety**: Price movements trigger liquidations when positions become risky
* **Redemption Values**: Redeemers pay GREEN at $1 and receive collateral at oracle price
* **Stability Pool Profits**: Liquidation spreads are measured against oracle prices
* **Interest Rates**: Dynamic rates respond to GREEN's share of its reference pool

## How Ripe Finds a Price

```
Asset Price Request Flow:

Your Asset (e.g., a stock token or ETH)
    ↓
Ripe's price router
    ↓
1. Ask the priority sources, in order
2. No price yet? Ask the remaining sources
3. Use the first good price
    ↓
USD Value
```

A source that reverts or returns garbage doesn't block the ones behind it, and different sources cover different assets.

## Our Oracle Providers

These are the adapters Ripe can run. Which are live is on [Params](https://params.ripe.finance).

### Chainlink

The industry standard. Decentralized node operators publish crypto and equity feeds; Ripe reads the latest round and checks that it's complete and fresh.

### Curve Pools

Prices stablecoins, Curve LP tokens, and GREEN straight from onchain pool state — real, tradeable liquidity, read live. It also records the GREEN reference-pool snapshots that drive [dynamic interest rates](02-borrowing.md#dynamic-interest-rates). Snapshots are taken as people use the protocol, so the rate signal reflects sustained conditions, not a single block.

### Pyth Network

Institutional publishers, sub-second updates, broad coverage. Every Pyth price comes with a confidence band; Ripe uses the price minus that band and rejects the price if the band is too wide.

### Stork Network

Signed price attestations from decentralized publishers, checked against their publish time.

### RedStone

A Chainlink-style adapter: same round and freshness checks, different data network.

### Blue Chip Yield Prices

Pricing for yield-bearing tokens from major lending protocols. The underlying asset's price comes from the router; the adapter handles the exchange rate on top:

* **Aave v3, Compound v3**: rebasing tokens — priced at the underlying's price, no snapshots needed
* **Morpho, Morpho v2, Euler, Fluid**: ERC-4626 shares — duration-weighted snapshots of the share rate
* **Moonwell**: its exchange rate, snapshotted the same way

Example: a Morpho USDC position. USDC is $1.00 from the router. The share rate is a weighted average over recent snapshots — say 1.05 USDC per share. The position is worth $1.05 per share.

Only the share rate is smoothed — a flash-loan spike barely moves the weighted average, and a sudden jump is throttled in gradually.

### Underscore Vault Pricing

ERC-4626 vault shares from Underscore, priced the same way: underlying price from the router, share rate from snapshots with their own freshness rules.

## Staleness Protection

Every feed has a freshness window set by governance, and each adapter checks it against its own clock:

* **Chainlink, RedStone**: the round's update time
* **Pyth, Stork**: the publish time
* **Curve**: always current — it reads pool state live
* **Blue Chip, Underscore**: snapshot age, using each feed's own delay and freshness settings

### Stock-Market Hours and Price Gaps

Stock feeds follow market hours. When the exchange closes, the last published price holds for as long as it's inside the feed's freshness window; when the market reopens, the new price lands in one step. Ripe doesn't read the issuer's "oracle paused" flag or a market calendar — it reads the token-level feed price, corporate-action multiplier already applied. Full treatment: [Stock Tokens on Ripe](00-stock-tokens.md#market-hours-and-weekend-gaps).

### What Happens When Prices Go Stale?

1. **A source goes stale**: Ripe moves to the next one
2. **Every source is unusable**: Ripe fails closed. It never substitutes an old cached price

### When an Account Cannot Be Valued

If one of your collateral assets loses its price, Ripe stops guessing about your whole position. Two things can be wrong, and they freeze you differently:

**The feed is stale or broken** — the usual case: a stock feed past its freshness window, a bad round. Every action that re-values your account reverts until a good price is back — borrowing, deposits, withdrawals, liquidation, deleverage — whether or not you have debt, and redemption skips you. A batch liquidation reverts for every account in it if one of them holds a stale asset. Repaying GREEN still works, and with no debt you can still withdraw the stale asset itself in full.

**The asset has no price source at all, or its vault is unbacked** — an issuer burned tokens out of the vault, say. Ripe quarantines your borrowing terms: the asset counts for zero, you can't borrow, and while you have debt you can't withdraw anything. You can still repay and add collateral, and liquidation, redemption, and deleverage skip your account. With no debt you can still withdraw your other assets; an unbacked token can't leave until it's backed again.

Everything resumes the moment the price — or the missing backing — is back, using the new price. Zero-LTV assets never trigger any of this — they don't back your loan, so they don't need a price.

## Governance and Safeguards

Adding a price source, changing priority, and adding or changing a feed all go through timelocked governance. Adapters can be disabled or replaced the same way, and stuck oracle update fees can be recovered.

## Trust Through Verification

Here's what your lending protocol won't tell you: they probably use one oracle. Maybe two if they're fancy.

Ripe checks every source governance turns on, in order. Primary source down? The next one is already answering. No delays. No manual intervention.

And when nothing can price an asset — a single-feed stock token over a long holiday, say — Ripe stops rather than guesses. Your position freezes, repay-only, until the truth comes back.

That's not a feature. That's survival.

***

_For technical implementation details, see:_

* [_Price Desk Technical Documentation_](https://ripe-finance.gitbook.io/ripe-developers/pricing/pricedesk) _- Ordered price routing_
* [_Chainlink Prices_](https://ripe-finance.gitbook.io/ripe-developers/pricing/chainlinkprices) _- Chainlink integration_
* [_Curve Prices_](https://ripe-finance.gitbook.io/ripe-developers/pricing/curveprices) _- AMM-based pricing and GREEN monitoring_
* [_Pyth Prices_](https://ripe-finance.gitbook.io/ripe-developers/pricing/pythprices) _- High-frequency oracle updates_
* [_Stork Prices_](https://ripe-finance.gitbook.io/ripe-developers/pricing/storkprices) _- Decentralized price attestations_
* [_Blue Chip Yield Prices_](https://ripe-finance.gitbook.io/ripe-developers/pricing/bluechipyieldprices) _- Yield token valuations_
* [_RedStone Prices_](https://ripe-finance.gitbook.io/ripe-developers/pricing/redstone) _- Chainlink-style adapter, different data network_
* [_Underscore Vault Prices_](https://ripe-finance.gitbook.io/ripe-developers/pricing/undyvaultprices) _- ERC-4626 vault share pricing_
