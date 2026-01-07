---
description: Your Money Working Overtime
---

# The Endaoment: Your Money Working Overtime

Every protocol has a treasury. Most sit on millions doing nothing.

The Endaoment puts every dollar to work. Bond proceeds from [bond sales](../governance-and-economics/03-bonds.md) don't collect dust—they're deployed across DeFi earning yield, providing liquidity, and defending [GREEN](01-green-stablecoin.md)'s peg. It's a treasury that compounds while others sleep.

This is what happens when you design a treasury to grow wealth, not just hold it.

## Why The Endaoment Exists

Traditional DeFi protocols face a critical challenge: they must choose between maintaining large idle treasuries for stability or deploying capital for growth. The Endaoment solves this dilemma by creating a dynamic treasury that:

1. **Actively grows protocol wealth** through sophisticated yield strategies
2. **Defends GREEN's stability** with algorithmic market operations
3. **Operates autonomously** without manual treasury management
4. **Creates sustainable revenue** that benefits all protocol participants

## The Three-Contract Architecture

Three contracts. One treasury system. Here's how they work together:

```
┌─────────────────────────────────────────────────────────────┐
│                      ENDAOMENT SYSTEM                       │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  ┌───────────────┐   ┌───────────────┐   ┌───────────────┐  │
│  │EndaomentFunds │   │   Endaoment   │   │ EndaomentPSM  │  │
│  │  (Treasury)   │◄──│ (Orchestrator)│──►│    (PSM)      │  │
│  └───────────────┘   └───────────────┘   └───────────────┘  │
│                                                             │
│    Holds all           Swaps, LP,          GREEN/USDC       │
│    protocol            yield, peg          conversions      │
│    assets              stabilization       + USDC yield     │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

**EndaomentFunds** — The Vault
Where the money lives. Holds all protocol-owned assets. Only Endaoment can withdraw — nobody else touches these funds.

**Endaoment** — The Operator
The brains of the operation. Swaps tokens, provides liquidity, deploys yield strategies, runs the GREEN stabilizer. This is where treasury management actually happens.

**EndaomentPSM** — The Peg Defender
Your direct line to swap GREEN ↔ USDC at $1. Rate-limited to prevent games, but always ready to defend the peg through arbitrage.

## How Value Flows Through The Endaoment

### The Treasury Flywheel

```
Bond Sales → Treasury Growth → Yield Generation → Protocol Strength
     ↑                                                      ↓
     └──────────── More User Confidence ←───────────────────┘
```

Every stablecoin that enters through [bond sales](../governance-and-economics/03-bonds.md) becomes productive capital that:

* Earns yield across multiple DeFi protocols
* Provides liquidity for GREEN trading
* Backs the protocol during market stress
* Funds operations without token inflation

## Core Capabilities

### 1. Intelligent GREEN Stabilization

The Endaoment acts as GREEN's guardian, maintaining its $1 peg through automated market operations:

**The 50/50 Rule**: The system monitors GREEN's ratio in Curve pools, targeting perfect balance with paired stablecoins.

**When GREEN Weakens** (trades below $1):

* Detects when GREEN exceeds 50% of the pool
* Removes excess GREEN liquidity
* Burns GREEN to reduce circulating supply
* Market forces push price back to $1

**When GREEN Strengthens** (trades above $1):

* Detects when GREEN falls below 50% of the pool
* Adds GREEN liquidity to increase availability
* May mint new GREEN (tracked as debt)
* Market forces bring price back to $1

This creates a self-balancing system where GREEN maintains its peg without manual intervention.

### 2. Peg Stability Module (PSM)

Want to swap GREEN for USDC at exactly $1? That's the PSM. No slippage, no DEX drama — just a clean 1:1 conversion.

**What You Can Do**:

* **Mint GREEN**: Deposit USDC → Receive GREEN at 1:1 (can auto-wrap to sGREEN)
* **Redeem GREEN**: Burn GREEN → Receive USDC at 1:1 (accepts sGREEN too)

**How It Defends the Peg**:

```
GREEN below $1?
→ Arbitrageurs buy cheap GREEN
→ Redeem via PSM for full $1 USDC
→ Buying pressure restores peg

GREEN above $1?
→ Arbitrageurs mint GREEN for $1 USDC
→ Sell on market for profit
→ Selling pressure restores peg
```

**Guardrails**:

* Rate limits prevent manipulation
* Optional fees on mint/redeem
* Allowlisting available for controlled rollouts
* Underscore Protocol gets unlimited access (operational needs)

**Idle USDC? Still Working.**

The PSM doesn't let reserves sit around doing nothing. Excess USDC auto-deposits into yield vaults, pulls back when needed for redemptions. Earning returns while waiting for arbitrage opportunities.

Self-sustaining peg defense. That's the idea.

### 3. Multi-Strategy Yield Engine

The Endaoment leverages **[Underscore Protocol](https://underscore.finance/)** — an advanced infrastructure that provides standardized integrations (called "Legos") with DeFi protocols. This partnership enables both programmatic treasury management today and AI-driven optimization in the future.

**How Underscore Powers the Endaoment:**

* **Unified Interface**: Every yield strategy uses the same standardized commands, whether deploying to Aave or Uniswap
* **Registry-Based Discovery**: New protocol integrations automatically become available without contract upgrades
* **AI-Ready Architecture**: Designed from day one to enable AI agents to analyze and execute complex treasury strategies

**Active Strategies Include:**

* **Lending Protocols**: Earning interest on Aave, Morpho, Euler, Fluid, Compound
* **Automated Market Makers**: Providing liquidity on Aerodrome, Uniswap, Curve
* **Liquid Staking**: Capturing ETH staking rewards
* **Concentrated Liquidity**: Maximizing capital efficiency

**Smart Allocation**: The system continuously optimizes between strategies based on:

* Real-time yield comparisons
* Risk-adjusted returns
* Liquidity needs
* Market conditions

**Future AI Integration**: While currently operating through programmatic rules, the Endaoment's architecture is built to support AI treasury managers that could dynamically rebalance across integrated protocols, finding optimal yield opportunities 24/7.

### 4. Strategic Partnership Programs

The Endaoment enables win-win liquidity partnerships:

**For Partners:**

* Co-invest alongside protocol treasury
* Share in liquidity provision rewards
* Reduce impermanent loss through diversification
* Access protocol-generated GREEN liquidity

**For Ripe Protocol:**

* Deepen liquidity without dilution
* Establish ecosystem relationships
* Expand market presence
* Generate additional revenue streams

## Where Protocol Revenue Goes

Every fee the protocol earns? It goes somewhere useful.

### The Revenue Split

Borrowing fees and interest get split two ways:

* **RIPE Buybacks**: A portion buys RIPE off the market (good for RIPE holders)
* **sGREEN Yield**: The rest flows to [sGREEN](../earning-and-rewards/01-sgreen.md) holders

The split ratio is adjustable by governance. More borrowing = more revenue = both tokens benefit. See [RIPE Value Accrual](../governance-and-economics/01-ripe-tokenomics.md#ripe-value-accrual-real-revenue-real-buybacks) for the full breakdown.

### Treasury Yields (Future)

Right now, all Endaoment earnings stay in the treasury — compounding, growing the war chest.

But governance can change that. Future options include directing treasury yields to:

* **RIPE stakers** — reward the long-term believers
* **sGREEN holders** — boost returns beyond stability pool earnings

Real yield from real operations. Not inflation. That's the difference.

## What Sets The Endaoment Apart

Unlike traditional protocol treasuries that rely entirely on manual decisions, the Endaoment combines automated mechanisms with strategic oversight. It doesn't just hold assets — it actively manages them to:

1. **Generate sustainable yield** without taking excessive risks
2. **Defend GREEN's stability** through market-based mechanisms
3. **Grow protocol wealth** in both bull and bear markets
4. **Operate transparently** with all actions verifiable onchain

## The Compound Effect

Here's what most people miss about treasuries: they're not just backstops. They're growth engines.

Every bond sold adds fuel. Every yield harvest compounds. Every market intervention strengthens the peg. The Endaoment doesn't just protect the protocol—it makes it unstoppable.

While other treasuries measure success in dollars held, the Endaoment measures it in dollars earned. That's the difference between surviving and thriving.

The treasury is working. The yields are compounding. The protocol is growing.

Are you in?

***

_For technical implementation details, see the_ [_Endaoment Technical Documentation_](https://ripe-finance.gitbook.io/ripe-developers/treasury-and-rewards/endaoment)_._
