---
description: Your Money Working Overtime
---

# The Endaoment: Your Money Working Overtime

Every protocol has a treasury. Most sit on millions doing nothing.

Payments from [bond sales](../governance-and-economics/03-bonds.md) and [Reserve Engine acquisitions](../governance-and-economics/04-reserve-engine.md) become Endaoment treasury assets. Switchboard-authorized actions can deploy those assets for yield, liquidity, or [GREEN](01-green-stablecoin.md) peg support.

This is what happens when you design a treasury to grow wealth, not just hold it.

## Why The Endaoment Exists

Traditional DeFi protocols face a critical challenge: they must choose between maintaining large idle treasuries for stability or deploying capital for growth. The Endaoment solves this dilemma by creating a dynamic treasury that:

1. **Can deploy treasury assets for potential yield** through configured strategy integrations
2. **Can support GREEN's stability** through authorized market operations
3. **Executes through Switchboard-authorized actions** rather than an autonomous scheduler
4. **Retains proceeds as treasury assets** for future authorized operations

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
Bond / Reserve Payments → Treasury Custody → Authorized Operations → Protocol Resources
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
* The resulting balance change can help move the market price toward $1

**When GREEN Strengthens** (trades above $1):

* Detects when the normalized paired-asset balance exceeds the GREEN balance
* Adds a bounded amount of GREEN liquidity, using treasury GREEN first
* May mint new GREEN, tracked as pool debt and limited by the configured ceiling
* The resulting liquidity can help move the market price toward $1

Each adjustment requires a Switchboard-authorized transaction and must pass the configured pool and debt controls; the contracts do not schedule or execute these actions autonomously.

### 2. Peg Stability Module (PSM)

Want to swap GREEN for its configured six-decimal reserve stablecoin around $1? That's the PSM. No DEX drama — just a direct conversion when the relevant direction is enabled.

**What You Can Do**:

* **Mint GREEN**: Deposit the reserve stablecoin → Receive GREEN (can auto-wrap to sGREEN)
* **Redeem GREEN**: Burn GREEN → Receive the reserve stablecoin (accepts sGREEN too)

**How It Can Defend the Peg**:

These illustrative flows assume the relevant direction is enabled, reserve-token pricing is usable, ordinary interval and sender-allowlist checks pass, and the recipient is valid. Redemption also requires sufficient reserve liquidity.

```
GREEN below $1?
→ Arbitrageurs buy cheap GREEN
→ Redeem via PSM for the configured reserve quote
→ Buying pressure can help restore the peg

GREEN above $1?
→ Arbitrageurs mint GREEN for the configured reserve quote
→ Sell on market for a potential gross spread before fees and execution costs
→ Selling pressure can help restore the peg
```

**Guardrails**:

* Interval limits bound ordinary conversion volume
* Optional fees on mint/redeem
* Allowlisting available for controlled rollouts
* A recognized Underscore-vault recipient bypasses the ordinary allowlist, fee, and interval controls, but minting remains bounded by submitted input and redemption by available reserves

**Idle Reserves? Still Working.**

When a compatible yield position and automatic deposits are configured, the PSM can deposit excess reserves into that vault and pull them back for redemptions. Without that configuration, it simply holds the reserve asset.

A bounded peg-support path. That's the idea.

### 3. Registered Strategy Integrations

The Endaoment resolves configured integration adapters, called **Legos**, through the [Underscore Protocol](https://underscore.finance/) registry. A separately authorized action can use a registered adapter to deposit into or withdraw from a yield position, swap tokens, add or remove liquidity, or claim incentives.

Registry availability alone does not deploy treasury assets. Each operation requires its own authorized transaction and must pass the Endaoment's adapter access, custody, amount, and transaction-specific minimum checks. Outcomes remain subject to the selected integration and market risk.

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

Configured daowry and borrowing-triggered flushed interest are split two ways:

* **Governance Allocation**: A portion is transferred in GREEN to governance for potential separately authorized buyback use
* **sGREEN Backing**: The remainder is transferred to [sGREEN](../earning-and-rewards/01-sgreen.md)

The split ratio is adjustable by governance. CreditEngine performs the GREEN transfers but does not purchase RIPE; any later buyback requires a separate authorized action. See [RIPE Value Accrual](../governance-and-economics/01-ripe-tokenomics.md#ripe-value-accrual-revenue-allocation-and-potential-buybacks) for the full breakdown.

### Treasury Yields

Endaoment earnings remain treasury assets unless governance separately authorizes another transfer or use. They are not part of CreditEngine's sGREEN distribution or governance buyback allocation.

## What Sets The Endaoment Apart

Unlike a passive protocol treasury, the Endaoment combines contract-enforced mechanisms with Switchboard-authorized execution. Authorized actions can:

1. **Deploy assets for potential yield** through configured strategies, subject to strategy risk
2. **Support GREEN's peg** through authorized market-based mechanisms
3. **Manage treasury assets** across changing market conditions
4. **Operate transparently** with all actions verifiable onchain

## The Compound Effect

Here's what most people miss about treasuries: they're not just backstops. They're growth engines.

Bond payments, realized yield, and successful market operations can expand the resources available to the treasury and support future authorized actions.

While other treasuries measure success in dollars held, the Endaoment measures it in dollars earned. That's the difference between surviving and thriving.

Outcomes depend on market performance and authorized execution; no strategy guarantees yield or a fixed GREEN market price.

Are you in?

***

_For technical implementation details, see the_ [_Endaoment Technical Documentation_](https://ripe-finance.gitbook.io/ripe-developers/treasury-and-rewards/endaoment)_._
