---
description: When Leverage Goes Wrong (But Not Too Wrong)
---

# Liquidations: When Leverage Goes Wrong (But Not Too Wrong)

Most protocols liquidate everything when you cross the line. Position worth $10,000? Debt at $9,001? They take it all. You get nothing.

Ripe calculates a target intended to restore safer account health. A limited shortfall may leave collateral behind, while a severe shortfall can produce a full-debt target and exhaust all eligible collateral without fully clearing the debt.

Multiple protection paths. Targeted liquidation. Proactive deleveraging available. This isn't charity — it's math. Preserving collateral when conditions permit helps keep borrowers and the protocol healthy.

## Executive Summary

**Key Points:**

* 🛡️ **Separate protection paths**: Proactive deleverage and redemption are separate entry points; once liquidation is submitted, configured collateral can settle through Stability pools and/or Dutch auctions
* 📊 **Target-based**: Limited shortfalls may produce a partial target; severe shortfalls can produce a full-debt target and exhaust eligible collateral without fully clearing the debt
* 💰 **Fair pricing**: Configured liquidation fees on volatile assets
* ⚡ **Permissionless execution**: Eligible callers may submit liquidation transactions and receive configured keeper compensation
* 🎯 **Proactive options**: [Deleverage](05-deleverage.md) your position before liquidation with zero penalties

**Quick Visual: The Liquidation Flow**

```
Before liquidation — separate actions when eligible
├─ Deleverage: selected collateral reduces debt
└─ Redemption: GREEN is exchanged against eligible collateral

At or below the liquidation threshold
└─ An eligible caller submits liquidation
   ├─ Configured Stability settlement, when compatible and funded
   └─ Remaining auction-enabled collateral → Dutch auction
```

**What This Means For You:**

* ✅ Deleverage proactively to avoid liquidation entirely
* ✅ Keep remaining collateral when the target can be met without exhausting it
* ✅ Lower fees than other protocols
* ✅ Designed to reduce large one-shot collateral sales
* ✅ Fair, transparent process

## Quick Navigation

**Understanding the Basics:**

* [Why Liquidations Matter](04-liquidations.md#why-liquidations-matter) - Protocol safety and borrower protection
* [Risk Zones](04-liquidations.md#understanding-the-risk-zones) - Three thresholds and visual guide
* [Deleverage First](05-deleverage.md) - Proactive debt reduction (separate page)
* [Redemption Buffer](04-liquidations.md#the-redemption-buffer) - Your first line of defense

**The Liquidation Process:**

* [Phase 1: Stability Pools](04-liquidations.md#phase-1-stability-pool-swaps) - Instant liquidity for volatile assets
* [Phase 2: Dutch Auctions](04-liquidations.md#phase-2-dutch-auctions) - Time-based discounts

**Advanced Topics:**

* [Liquidation Economics](04-liquidations.md#liquidation-economics) - Fees and calculations
* [Keeper Network](04-liquidations.md#the-keeper-network) - Permissionless execution
* [Bad Debt Handling](04-liquidations.md#what-if-bad-debt-occurs) - Last resort measures

***

## Why Liquidations Matter

### Protecting Protocol Solvency

Liquidations serve as the critical mechanism ensuring that [GREEN](01-green-stablecoin.md) remains fully backed. When [borrowing positions](02-borrowing.md) become undercollateralized due to collateral value drops or accumulated interest, the protocol must act to prevent bad debt accumulation. Without effective liquidations, GREEN could lose its peg, affecting GREEN and sGREEN holders.

### The Borrower-Friendly Approach

Unlike protocols designed around an automatic full-position close, Ripe calculates a health-restoration target and processes eligible collateral toward it. When conditions permit, this approach:

* **Preserves User Value**: Keep as much collateral as possible
* **Reduces Market Impact**: Smaller liquidations mean less selling pressure
* **Enables Recovery**: Partial liquidations allow positions to potentially recover
* **Maintains Fairness**: Configured, bounded fees rather than arbitrary penalties

## Understanding the Risk Zones

### The Three Critical Thresholds

Your position's safety depends on three key thresholds that work together to create graduated risk zones:

**1. Maximum LTV (Loan-to-Value)**

* Your borrowing limit as a percentage of collateral
* Example: 70% LTV on $10,000 = $7,000 maximum borrow
* Cannot borrow more once reached

**2. Redemption Threshold**

* Early warning system before liquidation
* GREEN holders can redeem against your position
* Provides market-based deleveraging opportunity
* Varies by asset and deployment configuration
* Example: At an assumed 77% threshold, GREEN is treated as a $1 debt-value input and oracle prices size the eligible collateral

**3. Liquidation Threshold**

* The danger zone where an account becomes eligible for forced liquidation
* Calculated as minimum collateral needed for your debt
* Varies by asset and deployment configuration
* Example: At an assumed 80% threshold with $8,000 debt, the account becomes liquidation-eligible when collateral ≤ $10,000

### How Risk Escalates: Visual Zone Map

Consider an illustrative position with $10,000 initial collateral and $6,000 debt, using assumed 70%, 77%, and 80% thresholds:

```
POSITION HEALTH VISUALIZATION (for $6,000 debt, ETH/BTC collateral)
←─────────────────────────────────────────────────────────────→
$10,000                    $8,571      $7,792     $7,500      $0
  YOU                        ↓           ↓           ↓
  ARE                   Max LTV     Redemption  Liquidation
  HERE                   (70%)        (77%)        (80%)

[════ SAFE ZONE ════][CAUTION][REDEMPTION][LIQUIDATION]
     ✅ Healthy        ⚠️ Warning  🚨 Danger    💀 Critical
```

**Zone Breakdown:**

**🟢 Zone 1: Healthy** (Collateral > $8,571)

* Below 70% LTV maximum
* Can still borrow more
* No third-party redemption or liquidation intervention is available

**🟡 Zone 2: Warning** (Collateral $8,571 - $7,792)

* At or beyond current max-borrow capacity, cannot borrow
* Still safe from redemption
* Borrowing-power collateral has no withdrawal room at the boundary; once debt exceeds current max-borrow capacity, ordinary withdrawals revert during whole-account debt housekeeping, including withdrawals of zero-LTV assets
* Time to add collateral or repay

**🟠 Zone 3: Redemption** (Collateral $7,792 - $7,500)

* Redemption threshold breached (77%)
* GREEN holders can redeem your collateral
* Acts as automatic deleveraging

**🔴 Zone 4: Liquidation** (Collateral ≤ $7,500)

* Liquidation threshold breached (80%)
* Multi-phase liquidation becomes eligible
* Execution still requires a submitted liquidation transaction

*Note: These values illustrate how the zones relate; see [RIPE Params](https://params.ripe.finance) for live terms.*

_For a detailed explanation of how these thresholds work together, see_ [_Understanding Three Thresholds_](02-borrowing.md#how-thresholds-work-together-a-visual-guide) _in the borrowing documentation._

## The Redemption Buffer

### Your First Line of Defense

Before liquidation becomes possible, the redemption mechanism provides a unique protective buffer. When your position enters the redemption zone:

* **GREEN is treated as a $1 debt-value input** while oracle pricing and actual vault delivery determine the collateral amount and debt credit
* **No discount applied** - fair value exchange protects you from penalties
* **Reduces your debt** automatically as collateral is redeemed
* **May prevent liquidation** by improving your position health

Each redemption is capped by the debt reduction needed to reach the account's target based on its lowest applicable LTV and configured buffer. It does not proceed while the borrower is in liquidation or valuation quarantine, or when the borrower address is registered as an Underscore Earn vault.

This mechanism serves dual purposes: protecting borrowers through gradual deleveraging while helping maintain GREEN's $1 peg during market stress.

**Want to take action before redemption?** An owner can use the [specific-assets deleverage route](05-deleverage.md#self-deleveraging-with-specific-assets) before the redemption zone when the account, asset, pricing, and authorization checks pass.

## The Liquidation Process

When liquidation becomes necessary, Ripe uses a two-phase approach designed to minimize impact while pursuing debt repayment.

If a debt-bearing collateral balance has no usable price or is nominally present in a vault with no usable backing, the account is quarantined and liquidation is declined. Ordinary GREEN repayment remains available while pricing or backing is restored.

AuctionHouse liquidation and ordinary credit redemption also skip a borrower address registered as an Underscore Earn vault. This exception follows the borrower address; it does not apply to an ordinary user merely because that user deposited an Earn-vault share as collateral.

### Before Liquidation: Deleverage

Before your position reaches liquidation, you have options to reduce debt with **zero penalties**:

* **[Deleverage](05-deleverage.md)** your position using GREEN, sGREEN, or stablecoins
* Burns your stability pool deposits to reduce debt directly
* Transfers configured stable-side collateral to Endaoment and credits debt at the collateral's oracle-valued amount
* No liquidation fees, no discounts — just debt reduction

This is handled by the separate [Deleverage](05-deleverage.md) system and can be triggered by you, delegated addresses, or third parties when you're in the redemption zone.

**Key Point**: GREEN, sGREEN, and assets configured for Endaoment settlement are skipped by liquidation itself and handled through deleveraging. Liquidation processes assets configured for Stability settlement and/or auctions.

### What Happens When Liquidation Starts

When an eligible liquidation call is submitted after your position reaches or crosses the liquidation threshold:

1. **Position enters `inLiquidation` state**
   * You are blocked from taking new borrows
   * All ordinary withdrawals are blocked, including zero-LTV assets
   * You can still repay debt to exit liquidation
   * Your volatile collateral becomes eligible for processing

2. **Liquidation fees are calculated once per episode**
   * The configured base liquidation fee and bounded keeper fee are assessed only when the account first enters liquidation
   * The combined base and keeper fees are capped by available collateral surplus; the keeper fee is reduced first, and both fees can fall to zero when surplus is insufficient
   * Stability-pool spread can cover at most the nominal base fee. Any unpaid base fee and the keeper fee are added to debt before repayment settlement; a productive keeper receives the keeper reward in GREEN or, optionally, sGREEN
   * A first pass that repays nothing and starts no auction is economically inert and earns no keeper fee

3. **Two-phase asset processing begins**
   * Eligible collateral is processed toward the calculated repayment target
   * You keep any collateral that remains, but severe shortfalls can exhaust eligible collateral

4. **Retrying or exiting liquidation mode**
   * An outstanding auction owns the current pass; another liquidation pass waits until no auction remains
   * A retry within the same episode does not assess the base or keeper fee again
   * Repay until debt is zero or no greater than current max-borrow capacity to exit `inLiquidation`; merely moving outside the liquidation threshold is not enough
   * Once debt health is restored, normal operations resume

### Phase 1: Stability Pool Swaps

The protocol engages [stability pools](../earning-and-rewards/02-stability-pools.md) for instant liquidity on your volatile assets (ETH, WBTC, etc.):

**How Pool Swaps Work**

1. Your collateral (ETH, WBTC, etc.) needs liquidation
2. [Stability pools](../earning-and-rewards/02-stability-pools.md) hold GREEN LP tokens and [sGREEN](../earning-and-rewards/01-sgreen.md)
3. Pool assets swap for your collateral at the liquidation discount
4. Pool participants get discounted assets, you avoid market dumps

Before collateral moves, the Stability vault must confirm that the settlement cohort can accept the liquidation asset and has capacity. If that check fails, auction-enabled collateral can continue to the Dutch-auction route.

**The Win-Win Dynamic**

* You avoid harsh market conditions and slippage
* Pool depositors receive the configured liquidation spread
* Protocol maintains orderly liquidations
* No dependence on external market depth

**Additional Pool Benefits**

* Depositors earn RIPE rewards from the Stakers allocation
* GREEN holders can redeem against pool collateral for peg stability
* Flexible withdrawal lets depositors choose which assets to claim

**Special Note on Permissioned Assets** For regulated assets (tokenized securities, real estate):

* Dedicated permissioned pools with whitelisted participants
* Same swap mechanics but restricted access
* Enforces configured participant and recipient whitelist restrictions during settlement

_For deeper understanding of stability pool mechanics, see_ [_Stability Pools_](../earning-and-rewards/02-stability-pools.md)_._

### Phase 2: Dutch Auctions

For remaining auction-enabled collateral when configured pool routes do not complete the target:

**Time-Based Discounts**

* Auctions start at a configured discount
* Discounts increase over the purchase window to the configured maximum on the final purchasable block
* After any configured start delay, eligible buyers can purchase during the configured window at the current discount, subject to general, asset, whitelist, recipient, and delegation controls
* An eligible purchase settles immediately rather than waiting for the auction window to end

**Auction Mechanics**

* Each purchase is capped by the supplied GREEN, the request limit, the borrower's live debt, and available collateral
* GREEN payment is burned, reducing your debt
* An individual asset auction is removed when its collateral position is depleted; restoring account debt health removes all outstanding auctions
* After the purchase window expires, anyone can remove an otherwise active expired auction so liquidation can be retried
* Collateral not purchased remains in the borrower's vault

## Liquidation Economics

### Understanding Liquidation Fees

Liquidation fees serve multiple purposes in the ecosystem:

**Illustrative Fee Structure**

* Base liquidation fee is configured per asset. Example values could include:
  * Stablecoins (USDC, etc.): 5%
  * Blue-chip assets (ETH, WBTC): 10%
  * Medium volatility (AERO, etc.): 10%
  * Meme coins (CBDOGE, etc.): 12%
  * High volatility (VVV, WELL, DEGEN): 15%
* Keeper rewards: An additional configured fee for liquidation executors (1% in this example)
* Total impact: Your cost becomes others' profit opportunity

**Where Fees Go**

1. **Stability Pool Depositors**: Receive discounted collateral as compensation
2. **Keepers**: Earn rewards for monitoring and executing liquidations
3. **Auction Buyers**: Purchase collateral below market value
4. **Protocol**: No direct protocol extraction - all value flows to participants

### Targeted Liquidation Design

Ripe computes a target repayment intended to restore safer account health. A limited shortfall can produce a partial liquidation, while a deeply underwater account can produce a full-debt target. If eligible settlement collateral cannot meet that target, the process can exhaust all of it without restoring full debt health.

The target uses live debt, configured fees, collateral value, the account's lowest LTV, and any configured payback buffer. The collateral actually moved also depends on eligible assets, pricing, available Stability liquidity, and auction execution.

## The Keeper Network

### Open Execution Network

Keepers are independent operators that can monitor positions and submit liquidation transactions when accounts become eligible.

**How Keepers Work**

* Monitor positions for liquidation thresholds
* Submit eligible liquidations
* Earn configured rewards when a productive liquidation qualifies
* Compete to execute liquidations quickly and efficiently

**Why This Benefits You**

* **Potentially Faster Response**: Open execution lets multiple operators compete
* **Open Competition**: Open eligibility lets multiple qualifying callers compete
* **Lower-Loss Potential**: Faster action can reduce how far underwater positions become
* **Open Participation**: Any eligible caller can submit a transaction

Anyone can be a keeper — no special permissions are needed to submit an eligible liquidation. The contracts make execution permissionless but do not guarantee monitoring coverage or transaction timing.

## Why Ripe's System is Superior

### Quick Comparison

| Feature                | Traditional DeFi       | Ripe Protocol                |
| ---------------------- | ---------------------- | ---------------------------- |
| **Liquidation Amount** | Often entire position | Targeted amount; severe shortfalls can produce a full-debt target and exhaust eligible collateral |
| **Warning System**     | None                   | Deleverage + Redemption zones |
| **Liquidation Fee**    | 13-50% penalty         | Configured liquidation spread |
| **Who Can Buy**        | MEV bots only          | Eligible configured pool participants and auction buyers |
| **Proactive Options**  | None                   | Deleverage with zero penalty |
| **Market Impact**      | Large dumps            | Phased, orderly process      |

### Borrower Protection Features

1. **Proactive Deleveraging**: Reduce debt before liquidation with zero fees
2. **Graduated Intervention**: Redemption buffer before liquidation
3. **Targeted Liquidations**: Limited shortfalls may be partial; severe ones can produce a full-debt target and exhaust eligible collateral without fully clearing the debt
4. **Configured Discounts**: Stability spreads and time-varying auction discounts follow their respective protocol settings
5. **Multiple Mechanisms**: Two phases provide redundancy

### System Stability Benefits

1. **Orderly Process**: Phased settlement can reduce cascading market sales
2. **Deep Liquidity**: Compatible, funded Stability pools can provide immediate settlement
3. **Market Independence**: Not reliant on external exchange depth
4. **Permissionless Execution**: Eligible callers can submit liquidation transactions
5. **Proven Resilience**: Designed for extreme market conditions

## What If Bad Debt Occurs?

Despite all protective mechanisms, extreme market conditions could potentially create bad debt (where liquidation proceeds don't fully cover the debt). The protocol has a clear resolution mechanism:

**Bond Sales for Recovery**: A qualifying [bond](../governance-and-economics/03-bonds.md) purchase can credit its oracle-valued payment against outstanding bad debt. In this process:

* The complete computed RIPE payout must fit the available bond allowance
* RIPE attributed to cleared bad debt is accounted outside ordinary bond-distribution usage
* Authorized cap administration must count that payout toward RIPE's protocol-wide 1 billion-token cap across all blockchains
* The purchase remains subject to bond, pricing, capacity, and minting controls

Authorized issuance administration must count RIPE attributed to this recovery path toward the protocol-wide cap. The mechanism does not guarantee that every amount of bad debt will be cleared.

## Liquidations That Don't Ruin Lives

Here's what actually matters:

**When others get liquidated**: They lose everything. Position gone. Starting over from zero.

**When you get liquidated on Ripe**: A limited shortfall may leave collateral and an active position; severe undercollateralization can consume all eligible collateral.

Even better: you can [deleverage](05-deleverage.md) before liquidation even happens — using your GREEN, sGREEN, or stablecoins to pay down debt with zero penalties. That's not an option on other protocols.

The protocol doesn't do this to be nice. It does it because borrowers who survive keep borrowing, keep paying interest, keep the system running. Your success is the protocol's success.

So go ahead. Take that loan. You've got deleverage, redemption buffers, stability pools, and auctions watching your back.

***

_For technical implementation details, see the_ [_AuctionHouse Technical Documentation_](https://ripe-finance.gitbook.io/ripe-developers/core-lending/auctionhouse)_._
