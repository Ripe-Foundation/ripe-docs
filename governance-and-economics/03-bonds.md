---
description: Trade Cash for Power
---

# Ripe Bonds: Trade Cash for Power

Every protocol needs capital. Most just sell tokens and spend it.

Ripe bonds turn your stablecoins into protocol-owned capital. Pay in, get newly minted RIPE at the epoch price, and your payment lands in the [treasury](../core-protocol/07-endaoment.md) — not in a marketing budget. Lock the RIPE and you get more of it. Have an activity booster? More again.

You're not funding an exit. You're funding the machine, and getting a piece of it.

> **Live terms live onchain.** The payment asset, epoch size and length, price range, lock bonus, boosters, and whether bonds are open at all vary by deployment and change over time. Every number on this page is an example. [Params](https://params.ripe.finance) is the source of truth.

Bonds settle instantly: RIPE arrives in your wallet or your governance-vault position the moment you buy. The [Reserve Engine](04-reserve-engine.md) is the other way to buy RIPE from the protocol — pay now, vest over time.

## Why Bonds Matter

Bonds solve a classic DeFi problem: how to build protocol-owned capital without renting mercenary liquidity. Every bond:

* **Grows the treasury** with assets governance can put to work
* **Distributes RIPE** in proportion to capital actually contributed
* **Rewards commitment** — longer locks and past contributions earn more
* **Can clear bad debt** if the protocol ever carries any (below)

## How Bonds Work

### Epochs

Bonds sell in epochs. Each epoch accepts a fixed amount of the payment asset and runs for a set number of blocks. First come, first served (governance can restrict bonding to a whitelist).

* **Sell-out**: when an epoch's capacity is gone (less than one whole unit left), the next one opens at the sold-out epoch's scheduled end plus a restart delay set by governance (if auto-restart is on)
* **Time**: if an epoch expires with capacity left, the next one rolls forward on schedule
* **Paused**: governance can close bonds entirely; check the app for the current epoch

### Price Moves Through the Epoch

Governance sets a minimum and maximum RIPE per payment token for each epoch. The rate starts at the minimum and rises linearly toward the maximum over the epoch — so waiting gets you more RIPE per dollar, at the risk that the epoch sells out first. If min and max are equal, the price is flat.

```
Start of Epoch → fewer RIPE per dollar (certainty of availability)
                      ↓
                 Time Passes
                      ↓
End of Epoch → more RIPE per dollar (if any capacity is left)
```

### Whole Units, Refunds, and Minimums

Bonds count whole payment tokens — 1 USDC, not 1.5. The fractional part of your payment is refunded, and so is anything above what the epoch can still accept. If you set a minimum RIPE payout and the full payout (base plus bonuses) comes in under it, the whole purchase reverts and you keep your money.

You can bond for another address only if that address has turned on `canAnyoneBondForUser`.

### Token Allocation

Bonds draw on their own RIPE allowance, separate from rewards and the Reserve Engine. It's an accounting limit, not a pile of escrowed tokens, and everything minted through it counts toward the protocol-wide [1 billion cap](01-ripe-tokenomics.md#supply-cap-one-billion-everywhere).

## Getting More RIPE Per Dollar

### Lock Bonus

Lock your RIPE in the [governance vault](02-governance.md) and the bond pays a bonus on top of the base amount. The bonus is linear between the vault's minimum and maximum lock:

```
Example: no minimum lock, 3-year max lock, 200% max bonus

No lock   →   0% bonus → 1x base
1 year    →  ~67%      → 1.67x
2 years   → ~133%      → 2.33x
3 years   →  200%      → 3x
```

* Pick any lock up to the maximum; ask for longer and it's capped there
* Locked RIPE goes straight into the governance vault, earning governance points and staker rewards from day one
* If your final lock comes in under the vault's minimum, it resets to zero: no lock bonus, and the RIPE — including any booster bonus — arrives unlocked in your wallet

### Bond Boosters

Governance can grant boosters to specific addresses — testnet contributors, for example — that add a further percentage of the base amount. A booster has three parts: a boost percentage, a number of **units** (one unit = one whole payment token), and an expiry block.

* The boost applies only if your whole purchase fits your remaining units — an oversized purchase gets no boost and uses no units
* Units used are gone; they don't refill while the grant is live (a fresh grant after expiry starts over)
* A booster comes with a minimum lock: ask for less and your lock is raised to the booster minimum (still capped at the vault maximum)
* Booster bonus and lock bonus are both computed from the base amount and added together

**Example:** 1,000 units, 200% booster.

* Bond 500 USDC → boosted; 500 units left
* Bond 1,500 USDC → doesn't fit; no boost, no units used
* Bond 500 USDC, then 1,000 later → the second bond doesn't fit the remaining 500 units; no boost

Example program: **Ripe Radness** rewarded testnet participants with 10%–200% boosters based on contribution, verified through Discord roles.

## Worked Examples

**Example 1 — all in.** Sarah bonds 5,000 USDC halfway through an epoch that runs from 2 to 6 RIPE per USDC, so the rate is 4 RIPE per USDC. She locks for 3 years and holds a 200% booster.

* Base: 20,000 RIPE
* 3-year lock: +40,000 (200% of base)
* Booster: +40,000 (200% of base)
* **Total: 100,000 RIPE, locked 3 years — $0.05 per RIPE**

**Example 2 — middle of the road.** James bonds 10,000 USDC three-quarters through the same epoch (5 RIPE per USDC), locks for 18 months, no booster.

* Base: 50,000 RIPE
* 18-month lock: +50,000 (100% of base)
* **Total: 100,000 RIPE, locked 18 months — $0.10 per RIPE**

## Bonds and Bad Debt

If governance has recorded bad debt — liquidations that didn't fully cover what was owed — bonds double as the recovery tool. Nothing changes for you: you get the full payout, bonuses included. Behind the scenes, the oracle value of your payment is credited against the recorded bad debt, up to the amount outstanding; the payment itself still goes to the treasury in full.

The RIPE that clears bad debt is tracked separately from ordinary bond distribution so anyone can see it, and it does not shrink the bond allowance. It still has to fit the allowance at purchase time, and it still counts toward the protocol-wide 1 billion cap. Bad debt costs every RIPE holder a little; governance has every reason to keep it at zero.

## Where the Money Goes

Every bond payment goes to the [treasury](../core-protocol/07-endaoment.md). From there it's governance's call: put it to work for yield, deepen GREEN liquidity, run market operations to defend the peg, or simply hold it in reserve.

```
Your Bond → Treasury → Yield / Liquidity / Peg Defense
     ↑                              ↓
     └──── Stronger GREEN ← Stronger Protocol
```

Unlike protocols that burn treasury on temporary incentives, your dollars stay protocol-owned.

## Time to Choose: Mercenary or Builder?

Here's the choice that matters:

Bond with a max lock? Up to 3x base in the example above. Add a booster? More again.

Wait for exchanges? Pay market price. No bonuses. No multipliers. Just hoping someone sells.

But this isn't really about the discount. It's about what happens to your money. Every dollar bonded becomes protocol-owned capital. Not exit liquidity for VCs. Not a marketing budget. Assets governance can put behind a stablecoin.

The protocol needs capital. You want tokens. Bonds make it happen.

***

_Check current epoch status and preview your payout in the Ripe app._

_For technical implementation details, see the_ [_BondRoom Technical Documentation_](https://ripe-finance.gitbook.io/ripe-developers/treasury/bondroom)_._
