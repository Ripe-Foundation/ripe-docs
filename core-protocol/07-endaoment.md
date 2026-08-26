---
description: Your Money Working Overtime
---

# The Endaoment: Your Money Working Overtime

Every protocol has a treasury. Most sit on millions doing nothing.

Payments from [bond sales](../governance-and-economics/03-bonds.md) and [Reserve Engine acquisitions](../governance-and-economics/04-reserve-engine.md) become Endaoment treasury assets. Switchboard-authorized actions can deploy those assets for yield, liquidity, or [GREEN](01-green-stablecoin.md) peg support.

This is what happens when you design a treasury to grow wealth, not just hold it.

## Why The Endaoment Exists

Traditional DeFi protocols face a critical challenge: they must choose between maintaining large idle treasuries for stability or deploying capital for growth. The Endaoment solves this dilemma by creating a dynamic treasury that:

1. **Actively grows protocol wealth** through sophisticated yield strategies
2. **Defends GREEN's stability** with algorithmic market operations
3. **Executes through Switchboard-authorized actions** rather than an autonomous scheduler
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
│    Holds all           Swaps, LP,          GREEN/reserve    │
│    protocol            yield, peg          conversions      │
│    assets              stabilization       + optional yield │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

**EndaomentFunds** — The Vault
Where the money lives. Holds all protocol-owned assets. Only Endaoment can withdraw — nobody else touches these funds.

**Endaoment** — The Operator
The brains of the operation. Swaps tokens, provides liquidity, deploys yield strategies, runs the GREEN stabilizer. This is where treasury management actually happens.

**EndaomentPSM** — The Peg Defender
When enabled, it provides conversions between GREEN and its configured six-decimal reserve stablecoin around $1. Availability and direction-specific controls are deployment configuration.

## How Value Flows Through The Endaoment

### The Treasury Flywheel

```
Bond / Reserve Payments → Treasury Growth → Yield Generation → Protocol Strength
             ↑                                                      ↓
             └──────────── More User Confidence ←───────────────────┘
```

Every payment token that enters through [bond sales](../governance-and-economics/03-bonds.md) or a [Reserve Engine acquisition](../governance-and-economics/04-reserve-engine.md) becomes treasury custody that authorized actions can use to:

* Earn yield through configured integrations
* Provide liquidity for GREEN trading
* Support the protocol during market stress

The Reserve Engine's RIPE allocation is separate from the payment asset: RIPE is minted only as vested claims are paid.

## Core Capabilities

### 1. Intelligent GREEN Stabilization

The Endaoment supports GREEN's peg through Switchboard-authorized market operations:

**The Balance Rule**: The stabilizer compares GREEN with the paired asset after normalizing both pool balances to the same value scale. It applies a configured fraction of the imbalance rather than forcing an exact balance in one transaction.

**When GREEN Weakens** (trades below $1):

* Detects when the normalized GREEN balance exceeds the paired-asset balance
* Removes a bounded amount of GREEN liquidity
* Burns recovered GREEN up to the pool's recorded debt
* Market forces push price back to $1

**When GREEN Strengthens** (trades above $1):

* Detects when the normalized paired-asset balance exceeds the GREEN balance
* Adds a bounded amount of GREEN liquidity, using treasury GREEN first
* May mint new GREEN, tracked as pool debt and limited by the configured ceiling
* Market forces bring price back to $1

Each adjustment requires a Switchboard-authorized transaction and must pass the configured pool and debt controls; the contracts do not schedule or execute these actions autonomously.

### 2. Peg Stability Module (PSM)

Want to swap GREEN for its configured six-decimal reserve stablecoin around $1? That's the PSM. No DEX drama — just a direct conversion when the relevant direction is enabled.

**What You Can Do**:

* **Mint GREEN**: Deposit the reserve stablecoin → Receive GREEN (can auto-wrap to sGREEN)
* **Redeem GREEN**: Burn GREEN → Receive the reserve stablecoin (accepts sGREEN too)

**How It Can Defend the Peg**:

These illustrative flows assume the relevant direction is enabled and the applicable reserve, interval, allowlist, and pricing checks pass.

```
GREEN below $1?
→ Arbitrageurs buy cheap GREEN
→ Redeem via PSM for the configured reserve quote
→ Buying pressure can help restore the peg

GREEN above $1?
→ Arbitrageurs mint GREEN for the configured reserve quote
→ Sell on market for profit
→ Selling pressure can help restore the peg
```

**Guardrails**:

* Rate limits prevent manipulation
* Optional fees on mint/redeem
* Allowlisting available for controlled rollouts
* A recognized Underscore-vault recipient bypasses the ordinary allowlist, fee, and interval controls, but minting remains bounded by submitted input and redemption by available reserves

**Idle Reserves? Still Working.**

When a compatible yield position and automatic deposits are configured, the PSM can deposit excess reserves into that vault and pull them back for redemptions. Without that configuration, it simply holds the reserve asset.

Self-sustaining peg defense. That's the idea.

### 3. Multi-Strategy Yield Engine

The Endaoment leverages **[Underscore Protocol](https://underscore.finance/)** — an advanced infrastructure that provides standardized integrations (called "Legos") with DeFi protocols. This partnership enables both programmatic treasury management today and AI-driven optimization in the future.

**How Underscore Powers the Endaoment:**

* **Unified Interface**: Every yield strategy uses the same standardized commands, whether deploying to Aave or Uniswap
* **Registry-Based Discovery**: Configured integrations can be resolved without an Endaoment contract upgrade; using them still requires an authorized action
* **AI-Ready Architecture**: Designed from day one to enable AI agents to analyze and execute complex treasury strategies

**Active Strategies Include:**

* **Lending Protocols**: Earning interest on Aave, Morpho, Euler, Fluid, Compound
* **Automated Market Makers**: Providing liquidity on Aerodrome, Uniswap, Curve
* **Liquid Staking**: Capturing ETH staking rewards
* **Concentrated Liquidity**: Maximizing capital efficiency

**Strategy Allocation**: Authorized allocation decisions can consider:

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

Partner-liquidity actions verify the expected LP token and actual custody received. Only LP tokens created by that action are split with the partner, and any provisional GREEN that was not used is recovered and burned.

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

Unlike a passive protocol treasury, the Endaoment combines contract-enforced mechanisms with Switchboard-authorized execution. Authorized actions can:

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
