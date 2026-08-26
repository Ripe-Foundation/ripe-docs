---
description: Pay now. Vest over time. Get more for waiting.
---

# RIPE Reserve Engine: RIPE That Vests

The Reserve Engine is the second way to buy RIPE straight from the protocol. Pay up front, pick how long you'll vest, and get a position that unlocks RIPE over time — with a bigger allocation the longer you wait. Your payment goes to the [treasury](../core-protocol/07-endaoment.md) the moment you buy; the RIPE is minted only as you claim it.

[Bonds](03-bonds.md) settle instantly and can clear bad debt. The Reserve Engine does neither; it has its own pricing, bonus, and budget.

> **Live terms live onchain.** The Reserve Engine is not deployed everywhere. Where it is, the payment token, epoch size, rate, vesting range, bonus, and budget vary by deployment and change over time. Every number on this page is an example. [Params](https://params.ripe.finance) is the source of truth.

## How a Purchase Works

1. **Preview.** Enter a payment amount and a vesting length. You get a quote: base RIPE, duration bonus, total, the current epoch's rate, and when your position starts claiming and matures.
2. **Buy.** Your transaction carries the quoted epoch, vesting length, a minimum RIPE out, and a deadline. If the epoch has rolled, the total fell below your minimum, or the deadline passed, it reverts — no partial fills.
3. **Position created.** Your payment goes to the treasury, your full allocation (base plus bonus) is reserved from the budget, and a vesting position is recorded in your name. No RIPE exists yet.

Positions belong to the buyer. You can't buy for someone else, and positions can't be transferred.

## Pricing: Epochs and the Controller

Time is sliced into epochs of a fixed number of blocks. Each epoch has one base rate (RIPE per payment token), a payment capacity, a minimum purchase, and a vesting range. Those terms are locked in by the **first purchase** in the epoch: a config change before that first purchase applies to the epoch; one after it waits for the next.

Between epochs a controller adjusts the base rate from the previous epoch's demand, within bounds governance sets:

* **High utilization** (the epoch mostly sold) → fewer RIPE per token next epoch. The earlier in the epoch the demand came, the bigger the move.
* **Low utilization** → more RIPE per token.
* **Idle epochs** with no purchases → the rate steps back up each idle epoch, up to a limit.

Governance can also pin an exact base rate for an upcoming epoch, overriding the controller for that epoch only. And there's a ceiling: base rate plus the maximum duration bonus can never exceed the all-in rate governance sets.

## The Duration Bonus

Pick any vesting length between the epoch's minimum and maximum. Ask for more and it's capped at the maximum; ask for less, or nothing, and you get the minimum. The bonus is linear — zero at the minimum, the full bonus at the maximum — computed on your base RIPE and added to it.

```
Example: 4 RIPE per USDC, 3-month minimum, 12-month maximum, 50% max bonus

10,000 USDC, 3-month vest  → 40,000 base + 0      = 40,000 RIPE
10,000 USDC, 12-month vest → 40,000 base + 20,000 = 60,000 RIPE
```

## Two Limits

* **Epoch capacity** caps how much payment an epoch accepts.
* **Allocation budget** caps how much RIPE all Reserve Engine positions can reserve in total. Every purchase must fit its full allocation in the budget; claims don't refill it.

Neither is escrowed RIPE. They're accounting limits, and everything minted through them counts toward the protocol-wide [1 billion cap](01-ripe-tokenomics.md#supply-cap-one-billion-everywhere).

## Vesting: The Catch-Up Cliff

Every position records three blocks:

```
creation    = the block you bought
claim start = creation + the epoch's minimum vesting length
maturity    = creation + your chosen vesting length
```

RIPE vests linearly from creation to maturity, but nothing can be claimed before claim start. At claim start, everything that has vested so far becomes claimable at once — a catch-up, not a delayed start. If you chose the minimum length, claim start and maturity are the same block and the whole allocation lands then.

**Example:** 12-month vest with a 3-month minimum. Nothing for 3 months; then about a quarter of the allocation is claimable immediately, with the rest vesting through month 12.

## Claiming

Claim from any position once it has vested RIPE. Two options:

* **Direct** — RIPE is minted to your wallet.
* **Auto-deposit** — RIPE is minted and locked in the [governance vault](02-governance.md) in one step, with the lock you request (the vault applies its own minimum and maximum).

Positions live in the vesting contract, and claims go through whichever engine is currently registered with the protocol — not necessarily the one you bought from.

## Pauses

The engine and the vesting contract pause separately:

* **Engine paused** (or purchases switched off): no new purchases; claims keep working.
* **Vesting paused**: no claims, and no new purchases either.

Mint permissions and the RIPE token's own pause sit above both: if RIPE can't mint, nothing settles.

## Before You Buy

* The preview is a quote, not a reservation. It's re-checked at execution.
* Pick a vesting length you're happy to wait out. There's no early exit.

***

_For technical implementation details, see the_ [_Technical Documentation_](https://ripe-finance.gitbook.io/ripe-developers)_._
