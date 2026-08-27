---
description: Deposit stock tokens, ETH, stablecoins — borrow GREEN against all of it in one loan.
---

# Borrowing: One Loan, Your Whole Portfolio

Deposit your stock tokens and borrow GREEN against them without selling a share. Then add the rest of your portfolio — WETH, stablecoins, yield positions — and it all backs the same loan. One position. One blended rate. One number to watch.

Most lending protocols make you open a vault per asset. Five assets, five loans, five liquidation risks. Ripe treats your portfolio like a portfolio.

> **Live terms live onchain.** Assets, LTVs, rates, fees, limits, and which optional features are switched on vary by deployment and change over time. Every number on this page is an example. [Params](https://params.ripe.finance) is the source of truth.

## How Borrowing Works

Every asset you deposit into a collateral vault backs one [GREEN](01-green-stablecoin.md) loan. Your borrowing power is the sum of each asset's value times its LTV:

```
Borrowing power = Σ (asset value × asset LTV)
```

Two things don't count: positions in [Stability Pools](../earning-and-rewards/02-stability-pools.md), which are never collateral, and assets with a 0% LTV (locked RIPE, for example), which sit in your account but add no borrowing power.

Ripe prices every asset through its oracle layer. If any of your borrowing collateral has no usable price, you can't borrow or withdraw until it does — see [When the Price Goes Missing](00-stock-tokens.md#when-the-price-goes-missing).

## Weighted Debt Terms Explained

With more than one collateral type, your terms are weighted by each asset's share of your borrowing power — not its raw value. Safer assets with higher LTVs pull your terms toward their numbers.

**Example:**

```
Stock token:  $10,000 × 60% LTV = $6,000 borrowing power
WETH:         $5,000  × 70% LTV = $3,500 borrowing power
Stablecoin:   $10,000 × 80% LTV = $8,000 borrowing power

Total borrowing power = $17,500
```

```
Stock token weight: $6,000 / $17,500 = 34.3%
WETH weight:        $3,500 / $17,500 = 20.0%
Stablecoin weight:  $8,000 / $17,500 = 45.7%

Rates: stock token 8%, WETH 7%, stablecoin 5%
Your rate = (8% × 34.3%) + (7% × 20.0%) + (5% × 45.7%) ≈ 6.4%
```

The same weighting sets your redemption threshold, liquidation threshold, and origination fee.

## Key Safety Thresholds

Three numbers govern your position. Each varies by asset and blends the same way your rate does.

**1. Max LTV — your borrowing limit.** Borrow up to this share of your collateral value. 70% LTV on $10,000 of ETH = $7,000 max.

**2. Redemption threshold — the warning zone.** Once collateral ≤ debt ÷ redemption threshold, GREEN holders can pay down your debt at $1 per GREEN and take collateral at oracle price. No discount, no penalty — a forced deleverage at fair value, and a warning before [liquidation](04-liquidations.md).

**3. Liquidation threshold — the danger zone.** Once collateral ≤ debt ÷ liquidation threshold, anyone can trigger a [liquidation](04-liquidations.md). Ripe takes only what it needs to make the position healthy again. If the shortfall is severe, the target becomes the full debt and all liquidation-eligible collateral can go.

### How Thresholds Work Together: A Visual Guide

**Example:** $6,000 debt against ETH with 70% LTV, 77% redemption, 80% liquidation:

```
COLLATERAL VALUE (for $6,000 debt)
←─────────────────────────────────────────────────────────────→
$10,000                    $8,571      $7,792     $7,500      $0

[════ SAFE ZONE ════][CAUTION][REDEMPTION][LIQUIDATION]
     ✅ Healthy        ⚠️ Warning  🚨 Danger    💀 Critical

│                          │            │           │
│                          │            │           └─ Liquidation (80%)
│                          │            │               $6,000 ÷ 0.80 = $7,500
│                          │            │
│                          │            └─ Redemption (77%)
│                          │                $6,000 ÷ 0.77 = $7,792
│                          │
│                          └─ Max Borrow / LTV (70%)
│                              $6,000 ÷ 0.70 = $8,571
│
└─ Your collateral today: $10,000 (167% coverage)
```

**🟢 Safe zone (collateral above $8,571).** Healthy. You can borrow up to $7,000 total and withdraw whatever isn't needed. Withdrawals keep a 1% buffer: you can take out only what leaves your debt at or under about 99% of your max borrow capacity.

**🟡 Caution zone ($8,571 – $7,792).** Over max LTV. No new borrows and no withdrawals — every withdrawal re-checks your whole position, so even 0%-LTV assets stay put. Repay or add collateral.

**🟠 Redemption zone ($7,792 – $7,500).** GREEN holders can [redeem](04-liquidations.md#the-redemption-buffer) your collateral, and anyone can [deleverage](05-deleverage.md) you. Act now: repay, add collateral, or deleverage on your own terms.

**🔴 Liquidation zone (below $7,500).** Anyone can trigger a liquidation. Repaying still works — and still helps.

**In liquidation.** Once a liquidation starts, your account carries that state until your debt is back at or under your max borrow capacity — however you get there. Repay, add collateral, or let prices recover and then do anything that re-checks your position (a deposit, a claim). Leaving liquidation also cancels any open auctions on your collateral. While you're in it: no new borrows and no withdrawals, 0%-LTV assets included.

### Two Ways to Read the Same Numbers

LTV looks forward (debt as a share of collateral). Redemption and liquidation look backward: the collateral level a given debt has to stay above. Same numbers, read from the other end.

| Threshold       | Where the line sits                                    | Coverage at the line | Collateral at the line |
| --------------- | ------------------------------------------------------ | -------------------- | ---------------------- |
| **Max borrow**  | No new borrowing once collateral ≤ debt ÷ 0.70         | 143%                 | $8,571                 |
| **Redemption**  | GREEN holders can redeem once collateral ≤ debt ÷ 0.77 | 130%                 | $7,792                 |
| **Liquidation** | Anyone can liquidate once collateral ≤ debt ÷ 0.80     | 125%                 | $7,500                 |

Collateral figures are for the $6,000 debt used throughout this section; stay above the line, not on it.

Interest grows your debt; a falling market shrinks your collateral. Both move you toward the thresholds. Watch coverage, not just price.

## Dynamic Interest Rates

Your normal rate is the weighted base rate from your collateral mix. A dynamic adjustment sits on top of it, and only kicks in when GREEN is under peg pressure in its reference pool. Most of the time you pay the base rate.

**How the signal is built.** Every time someone uses the protocol, Ripe snapshots the GREEN share of the reference pool. The signal is a duration-weighted average of recent snapshots, and each interval between snapshots counts at the lower of its two endpoints — so one spike can't raise your rate. An interval with too long a gap is dropped from the average; if the latest snapshot itself has gone stale, there's no signal at all and you pay the base rate.

**What happens above the trigger.** When the signal reaches the danger trigger (say, 60% GREEN), two boosts apply:

1. **Ratio boost** — multiplies your base rate. It scales from a minimum at the trigger to a maximum at 100% GREEN.
2. **Danger boost** — adds a fixed amount per block spent in danger. Danger time only grows while both ends of an interval are over the trigger, and only resets after a sustained safe window. Mixed or stale intervals neither add nor erase it.

A hard cap bounds the total, no matter how long the pool stays imbalanced.

**Example:**

```
Base rate:                                   5%
Ratio boost at 70% GREEN (100% of base):    +5%
Danger boost after a long stretch:          +0.5%
Rate until your next refresh:               10.5% (capped by the max rate)
```

**When it applies to you.** Your rate is stored on your loan and only refreshes when you borrow, repay, or your position is re-checked (a deposit, withdrawal, or claim). A change in the pool alone doesn't rewrite dormant debt — in either direction.

### Underscore Earn Vault Integration

[Underscore](https://underscore.finance/) Earn vaults are automated yield strategies. Some of them borrow GREEN from Ripe inside the strategy, and a registered Earn vault borrows on special terms:

* A governance-set discount on its weighted rate
* No origination fee
* No dynamic-rate adjustment
* Skipped by liquidation and redemption

**Example:** standard rate 6%, Earn vault rate 3% with a 50% discount, origination fee 0%.

These terms follow the borrower address. The vault gets them when it borrows; you don't get them for depositing an Earn vault share as collateral. You benefit through the vault's strategy returns, on the vault's own terms.

## Borrowing Limits

Every borrow has to clear each of these, and the amount is capped by the tightest one:

* **Collateral capacity** — the sum of value × LTV, minus what you already owe
* **Per-user debt ceiling** — the same cap for every account
* **Global borrower-debt limit** — caps total borrower debt; it limits new borrowing, not GREEN's supply
* **Max borrower count** — can stop a new account from opening its first loan; existing borrowers aren't affected
* **Interval limit** — caps how much you can borrow per window of blocks, so no one can drain capacity in a burst
* **Minimum debt** — a floor on new or increased loans, so tiny positions don't exist

If governance has turned on the one-action-per-block guard, the higher-risk actions — withdrawals, borrows, Stability Pool claims, and releasing a governance lock early — are limited to one per block per account. Every action you take stamps that slot, not just those four, so a deposit or a repayment earlier in the same block will also make a withdrawal or a borrow bounce.

## The Borrowing Experience

1. **Deposit collateral** into Ripe.
2. **Borrow** up to your available capacity.
3. **Pay the origination fee** — a one-time fee, weighted like your rate.
4. **Choose how to receive it.**

### Distribution Options

* **GREEN to your wallet.** The most flexible option — swap it, spend it, hold it.
* **sGREEN.** Your loan is wrapped into [sGREEN](../earning-and-rewards/01-sgreen.md) in the same transaction, so it starts earning immediately. If sGREEN yield beats your borrow rate, that's positive carry.
* **sGREEN into a Stability Pool.** Wrapped and deposited into the preferred [Stability Pool](../earning-and-rewards/02-stability-pools.md) in one step: sGREEN yield, liquidation proceeds, and [RIPE rewards](../earning-and-rewards/03-ripe-rewards.md). Least liquid, most going on.

Wrapping skips dust — an amount too small to wrap arrives as plain GREEN.

### Origination Fee

A one-time fee on each new borrow, deducted from what you receive. It's protocol revenue: by default all of it goes to sGREEN holders, though governance can split part of it into a governance allocation. Earn vaults are exempt.

**Example:** 0.25% fee. Borrow 10,000 GREEN → 25 GREEN fee → you receive 9,975 GREEN, and your debt is 10,000.

## Repayment

Repay on your schedule.

* **No term, no penalty.** Keep the loan as long as your collateral covers it; pay it down whenever you like.
* **Partial is fine.** Every payment lowers your debt — and your risk — immediately.
* **Pay with GREEN or sGREEN** straight from your wallet. Overpay and the excess comes back, as sGREEN by default. The app's MAX button pulls your full balance and refunds whatever wasn't needed.
* **Others can pay for you** if you've turned on `canAnyoneRepayDebt`. It's off by default.

Paying off in full is the cheapest transaction you'll make: with no debt left, there's no position to re-check.

## One Position That Understands a Portfolio

No more vault per asset. No more idle collateral. Your stock tokens, your ETH, and your stablecoins back one loan at one blended rate, with two warning zones before anything is forced. If GREEN needs defending, your rate rises — and it's capped. When it doesn't, you pay your base rate and nothing more.

***

_For technical implementation details, see the_ [_Credit Engine Technical Documentation_](https://ripe-finance.gitbook.io/ripe-developers/core/creditengine)_._
