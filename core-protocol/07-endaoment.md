---
description: Permissioned Treasury, Peg, and Liquidity Operations
---

# The Endaoment: Permissioned Treasury Operations

The Endaoment gives authorized protocol actions a way to deploy treasury assets across configured yield, liquidity, and [GREEN](01-green-stablecoin.md) stability operations. It is transaction-driven rather than an autonomous portfolio manager.

## Why The Endaoment Exists

The Endaoment provides permissioned infrastructure that can:

1. **Deploy treasury assets** through configured yield strategies
2. **Support GREEN's stability** with bounded market operations
3. **Execute governed treasury actions** through permissioned onchain operations
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
Accepted Bond or Reserve Engine Payment → EndaomentFunds Custody
                                               ↓
                          Retained Reserve or Authorized Operation
```

An accepted payment asset that enters through [bond sales](../governance-and-economics/03-bonds.md) or a [Reserve Engine acquisition](../governance-and-economics/04-reserve-engine.md) becomes an Endaoment treasury asset that can be used to:

* Enter a configured yield position
* Provide liquidity for GREEN trading
* Support authorized market operations
* Remain in reserve when no deployment action is taken

## Core Capabilities

### 1. GREEN Stabilization

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

The PSM exchanges GREEN with a reserve stablecoin around a $1 reference without using a DEX trade. Its reserve token is immutable after deployment and must use six decimals. Minting and redemption have different pricing rules, and the contract selects the ordinary or recognized-Underscore path from the **recipient address**, not from the sender.

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
* For an ordinary mint recipient, the sender can be subject to the mint allowlist, a fee is deducted, and interval capacity applies. The GREEN output is the lower of the reserve asset's oracle value and its par value
* A recognized Underscore mint recipient bypasses the mint allowlist, fee, and interval capacity, but uses the same lower-of-oracle-and-par pricing rule
* If sGREEN is requested but the GREEN output does not exceed the wrapping path's minimum size, the recipient receives GREEN directly instead
* For an ordinary redemption recipient, the sender can be subject to the redemption allowlist, interval capacity and a fee apply, and reserve output is the lower of the oracle-derived amount and par
* A recognized Underscore redemption recipient bypasses the redemption allowlist, interval capacity, and fee, and receives the higher of the oracle-derived amount and par
* Every redemption path remains limited by reserve liquidity, including whatever can be withdrawn from a configured yield position

Because recipient status controls the path, sending to a recognized Underscore vault can receive recognized treatment even when the sender is an ordinary address. A recognized vault sending to an ordinary recipient does not carry that treatment to the recipient.

**Optional Reserve Yield**

The PSM can be connected to a compatible yield position for its reserve asset. When configured for automatic deposits, idle reserves can be deployed there and withdrawn when redemption liquidity is needed. Without a configured position, the PSM simply holds the reserve asset.

### 3. Multi-Strategy Yield Engine

The Endaoment can use **[Underscore Protocol](https://underscore.finance/)** adapters (called "Legos") when a compatible registry and adapter are configured. Authorized calls identify the adapter and supply bounded inputs for a specific action.

**How Underscore Powers the Endaoment:**

* **Unified Interface**: Every yield strategy uses the same standardized commands, whether deploying to Aave or Uniswap
* **Registry-Based Discovery**: The Endaoment resolves configured adapter IDs through the Underscore registry
* **External Automation Compatibility**: An authorized system can select and submit actions, while the contract enforces its own permission, minimum-output, and custody checks

**Supported Action Types Can Include:**

* **Lending Protocols**: Positions in integrations such as Aave, Morpho, Euler, Fluid, or Compound
* **Automated Market Makers**: Liquidity positions in integrations such as Aerodrome, Uniswap, or Curve
* **Liquid Staking**: Configured liquid-staking positions
* **Concentrated Liquidity**: Configured concentrated-liquidity positions

**Transaction-by-Transaction Allocation**: An authorized caller can choose an action based on factors such as:

* Real-time yield comparisons
* Risk-adjusted returns
* Liquidity needs
* Market conditions

External automation can analyze opportunities and submit authorized transactions, but the Endaoment contract does not continuously scan markets or rebalance by itself.

### 4. Strategic Partnership Programs

An authorized partner-liquidity action pairs a partner asset with GREEN and specifies the contributed amount, minimum LP output, and expected LP token. The action can use GREEN already held by the treasury and provisionally mint additional GREEN for the intended contribution.

After the liquidity operation, the Endaoment verifies the reported asset contributions against actual treasury custody changes and verifies both the LP token and the LP amount received. Any provisional GREEN that was not actually contributed is recovered and burned, and only the provisional mint actually used is added to the pool's GREEN debt.

Only LP tokens created by that action are split. An external partner receives half, rounded down, and EndaomentFunds receives the remainder. If the Endaoment itself is supplied as the partner, the partner share is zero and EndaomentFunds receives all of the LP tokens. These accounting rules do not guarantee liquidity revenue, protection from impermanent loss, or any other investment outcome.

## Where Protocol Revenue Goes

### Credit Revenue Split

When a borrow realizes configured origination revenue and accrued borrowing interest, the Credit Engine splits that GREEN-denominated revenue between two destinations:

* **Governance Buyback Allocation**: A configured portion of GREEN revenue is sent to the governance recipient for separate RIPE buyback operations
* **sGREEN Backing**: The rest is sent to [sGREEN](../earning-and-rewards/01-sgreen.md), increasing GREEN backing per share

The split ratio is configurable. The Credit Engine performs the allocation but does not execute the market buyback itself. This flow is separate from Endaoment treasury assets and strategy proceeds. See [RIPE Value Accrual](../governance-and-economics/01-ripe-tokenomics.md#ripe-value-accrual-real-revenue-real-buybacks) for the full breakdown.

### Treasury Yields

Assets and proceeds from an Endaoment operation remain under the resulting contract custody and accounting until another authorized action moves them. The Endaoment does not promise a yield, automatically compound every position, or define a universal distribution of treasury strategy proceeds.

## What Sets The Endaoment Apart

Unlike a passive treasury address, the Endaoment exposes permissioned onchain actions that can:

1. **Enter or exit configured yield positions**
2. **Defend GREEN's stability** through market-based mechanisms
3. **Deploy or retain treasury assets** according to authorized decisions
4. **Operate transparently** with all actions verifiable onchain

## The Compound Effect

Here's what most people miss about treasuries: they're not just backstops. They're growth engines.

Bond and Reserve Engine payments can add treasury assets. Yield harvests can add value, and bounded market operations can support the peg. Each outcome depends on the action, integration, and market conditions.

The architecture makes treasury actions transparent and verifiable without claiming that idle assets are automatically deployed or that returns are guaranteed.

Are you in?

***

_For technical implementation details, see the_ [_Endaoment Technical Documentation_](https://ripe-finance.gitbook.io/ripe-developers/treasury-and-rewards/endaoment)_._
