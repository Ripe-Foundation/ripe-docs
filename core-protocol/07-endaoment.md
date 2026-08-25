---
description: Your Money Working Overtime
---

# The Endaoment: Your Money Working Overtime

Every protocol has a treasury. Most sit on millions doing nothing.

The Endaoment gives authorized protocol actions a way to deploy treasury assets across configured yield, liquidity, and [GREEN](01-green-stablecoin.md) stability operations. It is transaction-driven rather than an autonomous portfolio manager.

This is what happens when you design a treasury to grow wealth, not just hold it.

## Why The Endaoment Exists

Traditional DeFi protocols face a critical challenge: they must choose between maintaining reserves and deploying capital. The Endaoment provides permissioned infrastructure that can:

1. **Deploy treasury assets** through configured yield strategies
2. **Support GREEN's stability** with bounded market operations
3. **Executes governed treasury actions** through permissioned onchain operations
4. **Retain resulting assets and proceeds** under protocol treasury accounting

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
│    Primary treasury    Swaps, LP,          GREEN/reserve    │
│    protocol            yield, peg          conversions      │
│    assets              stabilization       + optional yield │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

**EndaomentFunds** — The Vault
Primary custody for bond payments and other treasury assets transferred there. Its transfer entry point sends assets only to the registered Endaoment contract.

**Endaoment** — The Operator
The execution layer. Authorized protocol actions use it to swap tokens, provide liquidity, deploy yield strategies, and run the GREEN stabilizer. This is where treasury management actually happens onchain.

**EndaomentPSM** — The Peg Defender
When enabled, it provides bounded conversions between GREEN and a configured reserve stablecoin. Minting and redemption have independent controls, so either direction can be available without assuming the other is.

## How Value Flows Through The Endaoment

### Enforced Treasury Flow

```
Accepted Bond Payment → EndaomentFunds Custody
                                  ↓
             Retained Reserve or Authorized Operation
```

An accepted payment asset that enters through [bond sales](../governance-and-economics/03-bonds.md) becomes an Endaoment treasury asset that can be used to:

* Enter a configured yield position
* Provide liquidity for GREEN trading
* Support authorized market operations
* Remain in reserve when no deployment action is taken

## Core Capabilities

### 1. Intelligent GREEN Stabilization

The Endaoment can support GREEN's peg through an authorized stabilizer transaction against a configured reference pool.

The stabilizer compares the pool's GREEN balance with the paired asset after both have been normalized into the same value scale. It applies a configurable fraction of the imbalance rather than forcing the pool to an exact 50/50 balance in one transaction.

**When the normalized paired-asset balance is larger**:

* Uses GREEN already held by the treasury first
* May mint additional GREEN and add it as one-sided liquidity
* Records newly minted GREEN as pool debt and respects the configured debt ceiling

**When the normalized GREEN balance is larger**:

* Removes a bounded amount of GREEN liquidity using available LP tokens
* Burns recovered GREEN up to the pool's recorded debt
* Keeps the LP quote, available liquidity, and tracked treasury position within the contract's safety checks

The operation is permissioned and transaction-driven; the contract does not run its own scheduler. Its calculation and debt accounting make the intervention repeatable without assuming every call can—or should—fully rebalance the pool.

### 2. Peg Stability Module (PSM)

The PSM exchanges GREEN with a configured reserve stablecoin around a $1 reference without using a DEX trade. The result is deliberately conservative: normal-user mint and redemption quotes are bounded by both the reserve asset's oracle value and par, then adjusted for any configured fee.

**What You Can Do**:

* **Mint GREEN**: Deposit the reserve stablecoin → receive GREEN, or have it deposited into sGREEN
* **Redeem GREEN**: Pay with GREEN or sGREEN → receive the reserve stablecoin

**How It Defends the Peg**:

```
GREEN below its reference?
→ Arbitrageurs can buy GREEN
→ Redeem through the PSM when redemption is enabled and capacity is available
→ Buying pressure can help restore the peg

GREEN above its reference?
→ Arbitrageurs can mint GREEN when minting is enabled and capacity is available
→ Sell GREEN in the market
→ Selling pressure can help restore the peg
```

**Guardrails**:

* Minting and redemption can be enabled independently
* Separate interval capacities limit ordinary minting and redemption
* Optional fees and allowlists can apply in either direction
* Redemption is always limited by available reserve liquidity, including configured yield positions
* Recognized Underscore vaults can receive favorable fee, interval, and pricing treatment, but still remain bounded by inputs, oracle pricing, and reserve availability

**Optional Reserve Yield**

The PSM can be connected to a compatible yield position for its reserve asset. When configured for automatic deposits, idle reserves can be deployed there and withdrawn when redemption liquidity is needed. Without a configured position, the PSM simply holds the reserve asset.

A reserve-backed peg tool. That's the idea.

### 3. Multi-Strategy Yield Engine

The Endaoment can use **[Underscore Protocol](https://underscore.finance/)** adapters (called "Legos") when a compatible registry and adapter are configured. Authorized calls identify the adapter and supply bounded inputs for a specific action.

**How Underscore Powers the Endaoment:**

* **Unified Interface**: Every yield strategy uses the same standardized commands, whether deploying to Aave or Uniswap
* **Registry-Based Discovery**: The Endaoment resolves configured adapter IDs through the Underscore registry
* **External Automation Compatibility**: An authorized system can select and submit actions, while the contract enforces its own permission, minimum-output, and custody checks

**Supported Action Types Can Include:**

* **Lending Protocols**: Earning interest on Aave, Morpho, Euler, Fluid, Compound
* **Automated Market Makers**: Providing liquidity on Aerodrome, Uniswap, Curve
* **Liquid Staking**: Capturing ETH staking rewards
* **Concentrated Liquidity**: Maximizing capital efficiency

**Transaction-by-Transaction Allocation**: An authorized caller can choose an action based on factors such as:

* Real-time yield comparisons
* Risk-adjusted returns
* Liquidity needs
* Market conditions

External automation can analyze opportunities and submit authorized transactions, but the Endaoment contract does not continuously scan markets or rebalance by itself.

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

Each partner-liquidity action specifies the contributed amount, minimum LP output, and expected LP token. The Endaoment verifies actual custody movement and the LP token received, splits only the LP created by that action, and burns any provisional GREEN mint that was not actually contributed.

## Where Protocol Revenue Goes

Every fee the protocol earns? It goes somewhere useful.

### The Revenue Split

Borrowing fees and interest get split two ways:

* **Governance Buyback Allocation**: A configured portion of GREEN revenue is sent to the governance recipient for separate RIPE buyback operations
* **sGREEN Yield**: The rest flows to [sGREEN](../earning-and-rewards/01-sgreen.md) holders

The split ratio is adjustable by governance. The Credit Engine performs the allocation but does not execute the market buyback itself. See [RIPE Value Accrual](../governance-and-economics/01-ripe-tokenomics.md#ripe-value-accrual-real-revenue-real-buybacks) for the full breakdown.

### Treasury Yields

Endaoment earnings can stay in the treasury to compound, while governance can also direct treasury yields to uses such as:

* **RIPE stakers** — reward the long-term believers
* **sGREEN holders** — boost returns beyond stability pool earnings

Real yield from real operations. Not inflation. That's the difference.

## What Sets The Endaoment Apart

Unlike a passive treasury address, the Endaoment exposes permissioned onchain actions that can:

1. **Enter or exit configured yield positions**
2. **Defend GREEN's stability** through market-based mechanisms
3. **Deploy or retain treasury assets** according to authorized decisions
4. **Operate transparently** with all actions verifiable onchain

## The Compound Effect

Here's what most people miss about treasuries: they're not just backstops. They're growth engines.

Bond payments can add treasury assets. Yield harvests can add value, and bounded market operations can support the peg. Each outcome depends on the action, integration, and market conditions.

The architecture makes treasury actions transparent and verifiable without claiming that idle assets are automatically deployed or that returns are guaranteed.

Are you in?

***

_For technical implementation details, see the_ [_Endaoment Technical Documentation_](https://ripe-finance.gitbook.io/ripe-developers/treasury-and-rewards/endaoment)_._
