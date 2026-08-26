---
description: A treasury that works.
---

# The Endaoment: Your Money Working Overtime

Every protocol has a treasury. Most sit on millions doing nothing.

The Endaoment puts every dollar to work. Payments for [bonds](../governance-and-economics/03-bonds.md) and [Reserve Engine](../governance-and-economics/04-reserve-engine.md) allocations become protocol-owned capital that governance deploys across DeFi for yield, GREEN liquidity, and [peg defense](01-green-stablecoin.md). It's a treasury built to grow, not just to hold.

> **Live terms live onchain.** Which integrations, pools, and PSM settings are switched on vary by deployment and change over time. Every number on this page is an example. [Params](https://params.ripe.finance) is the source of truth.

## Why The Endaoment Exists

Most treasuries choose: idle reserves for safety, or deployed capital for growth. The Endaoment does both:

1. **Grows protocol wealth** through yield strategies
2. **Defends GREEN** in the reference pool and through the PSM
3. **Acts on governance's call** — every deployment is a submitted, onchain transaction
4. **Keeps what it earns** to fund the next move

## The Three-Contract Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                      ENDAOMENT SYSTEM                       │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  ┌───────────────┐   ┌───────────────┐   ┌───────────────┐  │
│  │   Treasury    │   │   Endaoment   │   │      PSM      │  │
│  │    (Vault)    │◄──│  (Operator)   │──►│ (Peg Defender)│  │
│  └───────────────┘   └───────────────┘   └───────────────┘  │
│                                                             │
│    Holds the           Swaps, LP,          GREEN ↔ reserve  │
│    protocol's          yield, peg          stablecoin at $1 │
│    assets              stabilization       + reserve yield  │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

**The Vault** — Where the money lives. Only the operator can withdraw.

**The Operator** — The brains. Swaps, liquidity, yield strategies, the GREEN stabilizer.

**The Peg Defender** — Swap GREEN for its reserve stablecoin at $1, with limits to keep it honest.

## Where Value Comes From

```
Bond / Reserve Engine payments → Treasury → Yield & liquidity → Protocol strength
            ↑                                                        ↓
            └──────────────── More user confidence ←─────────────────┘
```

Every stablecoin paid for a bond or a Reserve Engine allocation becomes treasury capital (bonds mint their RIPE at purchase; the Reserve Engine mints only as it vests). The treasury also receives the non-GREEN assets a Stability Pool pays out in a liquidation, and the stablecoins a deleverage takes from a position. All of it can earn yield across DeFi, provide liquidity for GREEN trading, and back the protocol during market stress.

## Core Capabilities

### 1. Intelligent GREEN Stabilization

**The Balance Rule**: Compare GREEN with the paired stablecoin in the reference pool, on the same value scale. Whichever side is heavier, move a fraction of the gap — not the whole thing in one shot.

**When GREEN weakens** (too much GREEN in the pool):

* Pull GREEN liquidity out
* Burn it, up to the pool debt on record
* The pool rebalances and price drifts back toward $1

**When GREEN strengthens** (too little GREEN in the pool):

* Add GREEN liquidity, treasury GREEN first
* Mint the rest, recorded as pool debt, up to a ceiling
* Deeper pool; price drifts back toward $1

Governance submits each move; nothing runs on a timer. And a move only goes through if the treasury's net position doesn't get worse — the stabilizer never trades the treasury down to defend the peg.

### 2. Peg Stability Module (PSM)

No slippage, no DEX drama: the PSM is a direct swap between GREEN and its reserve stablecoin (one stablecoin, fixed at deployment) at $1, in whichever direction governance has turned on.

* **Mint GREEN**: Deposit the reserve stablecoin, receive GREEN 1:1 (ask for sGREEN and anything over 1 GREEN is wrapped for you)
* **Redeem GREEN**: Burn GREEN or sGREEN, receive the reserve stablecoin 1:1

```
GREEN below $1?
→ Arbitrageurs buy cheap GREEN
→ Redeem via the PSM for $1 of reserve stablecoin
→ Buying pressure restores the peg

GREEN above $1?
→ Arbitrageurs mint GREEN for $1 of reserve stablecoin
→ Sell on the market for the spread
→ Selling pressure restores the peg
```

**Guardrails**

* Per-interval limits on minting and redeeming
* Optional fees, and an optional allowlist for controlled rollouts
* Quotes use the reserve stablecoin's oracle price or 1:1, whichever gives you less; recognized Underscore vaults skip the limits, allowlist, and fee, and get the better quote on redemption
* Redemptions are capped by total reserves

**Idle reserves? Still working.** If governance has set a yield vault, the PSM sweeps its whole idle reserve balance into it and pulls funds back when a redemption needs them.

### 3. Yield and Liquidity Through Underscore

The Endaoment reaches DeFi through [Underscore Protocol](https://underscore.finance/), whose registry of standardized adapters ("Legos") gives every integration the same commands: deposit to or withdraw from a yield position, swap, add or remove liquidity, claim incentives. New integrations become usable as they're registered. Each deployment is its own governance-submitted transaction with whatever minimum-output checks governance sets, and carries the risk of the venue it goes to.

### 4. Strategic Partnership Programs

**For partners**: Co-invest alongside the treasury and share the LP position.

**For Ripe**: Deeper GREEN markets without dilution, plus a new revenue stream.

The mechanics: the partner's asset comes into the treasury, GREEN of equal value is paired with it (treasury GREEN first, minting the rest), the LP tokens are split 50/50, and any minted GREEN the pool didn't take is burned.

## Where Protocol Revenue Goes

### The Revenue Split

Origination fees and borrower interest can be split two ways:

* **sGREEN yield**: Flows to [sGREEN](../earning-and-rewards/01-sgreen.md) holders
* **Governance allocation**: Transferred in GREEN to governance for RIPE buybacks

The split is a governance setting, and the default sends 100% to sGREEN. The protocol only moves the GREEN; any RIPE buyback is a separate action by governance. See [RIPE Value Accrual](../governance-and-economics/01-ripe-tokenomics.md#ripe-value-accrual).

### Treasury Yields

Everything the Endaoment earns stays in the treasury unless governance directs it elsewhere — to RIPE stakers, to sGREEN holders, or back into the next strategy.

## The Compound Effect

Treasuries aren't just backstops. They're growth engines.

Every bond sold adds fuel. Every yield harvest compounds. Every stabilizer move leaves the treasury at least as strong as before, or it doesn't happen. Other treasuries measure success in dollars held. The Endaoment measures it in dollars earned.

The treasury is working. Are you in?

***

_For technical implementation details, see the_ [_Endaoment Technical Documentation_](https://ripe-finance.gitbook.io/ripe-developers/treasury-and-rewards/endaoment)_._
