---
description: When Leverage Goes Wrong (But Not Too Wrong)
---

# Liquidations: When Leverage Goes Wrong (But Not Too Wrong)

Ripe calculates a target intended to restore safer account health. A limited shortfall may leave collateral behind, while a severe shortfall can produce a full-debt target and exhaust all eligible collateral without fully clearing the debt.

Multiple protection paths. Targeted liquidation. Proactive deleveraging available. This isn't charity — it's math. Preserving collateral when conditions permit helps keep borrowers and the protocol healthy.

## Executive Summary

**Key Points:**

* 🛡️ **Separate protection paths**: Proactive deleverage and redemption are separate entry points; once liquidation is submitted, configured collateral can settle through Stability pools and/or Dutch auctions
* 📊 **Target-based**: Limited shortfalls may produce a partial target; severe shortfalls can produce a full-debt target and exhaust eligible collateral without fully clearing the debt
* 💰 **Configured economics**: An account-level liquidation-fee term is derived from the borrowing-power-weighted terms of eligible non-Stability collateral
* ⚡ **Permissionless execution**: Eligible callers may submit liquidation transactions and receive configured keeper compensation
* 🎯 **Proactive options**: [Deleverage](05-deleverage.md) your position before liquidation without liquidation or keeper fees

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
* ✅ Designed to reduce large one-shot collateral sales
* ✅ Fair, transparent process

## Quick Navigation

**Understanding the Basics:**

* [Why Liquidations Matter](04-liquidations.md#why-liquidations-matter) - Protocol safety and borrower protection
* [Risk Zones](04-liquidations.md#understanding-the-risk-zones) - Three thresholds and visual guide
* [Deleverage First](05-deleverage.md) - Proactive debt reduction (separate page)
* [Redemption Buffer](04-liquidations.md#the-redemption-buffer) - Your first line of defense

**The Liquidation Process:**

* [Phase 1: Stability Pools](04-liquidations.md#phase-1-stability-pool-swaps) - Configured pre-auction settlement
* [Phase 2: Dutch Auctions](04-liquidations.md#phase-2-dutch-auctions) - Configured time-based discounts

**Advanced Topics:**

* [Liquidation Economics](04-liquidations.md#liquidation-economics) - Fees and calculations
* [Keeper Network](04-liquidations.md#the-keeper-network) - Permissionless execution
* [Bad Debt Handling](04-liquidations.md#what-if-bad-debt-occurs) - Last resort measures

***

## Why Liquidations Matter

### Protecting Protocol Solvency

Liquidations are designed to defend protocol solvency and reduce bad-debt risk. When [borrowing positions](02-borrowing.md) become undercollateralized due to collateral value drops or accumulated interest, eligible liquidation routes can reduce debt and transfer collateral. They cannot guarantee complete backing in every market state: severe shortfalls can exhaust eligible collateral and leave residual borrower debt. Liquidation itself does not automatically add that amount to the protocol's separately administered recorded-bad-debt balance.

### The Borrower-Friendly Approach

Ripe calculates a health-restoration target and processes eligible collateral toward it. When conditions permit, this approach:

* **Can Preserve User Value**: Collateral can remain when the target is met before eligible assets are exhausted
* **Can Reduce Market Impact**: A limited repayment target can require less collateral processing
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
* GREEN holders can redeem against eligible collateral when the position, asset, and recipient checks pass
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
* GREEN holders can redeem eligible collateral when redemption is enabled and all checks pass
* Reduces debt when an eligible redemption is submitted

**🔴 Zone 4: Liquidation** (Collateral ≤ $7,500)

* Liquidation threshold breached (80%)
* Multi-phase liquidation becomes eligible
* Execution still requires a submitted liquidation transaction

*Note: These values illustrate how the zones relate; see [RIPE Params](https://params.ripe.finance) for live terms.*

_For a detailed explanation of how these thresholds work together, see_ [_Understanding Three Thresholds_](02-borrowing.md#how-thresholds-work-together-a-visual-guide) _in the borrowing documentation._

## The Redemption Buffer

### Your First Line of Defense

Before liquidation becomes possible, the redemption mechanism can provide a protective buffer. When redemption is enabled and your position enters the redemption zone:

* **GREEN is treated as a $1 debt-value input** while oracle pricing and actual vault delivery determine the collateral amount and debt credit
* **No discount applied** - fair value exchange protects you from penalties
* **Reduces your debt** when an eligible redemption transaction succeeds
* **May prevent liquidation** by improving your position health

Each redemption is capped by the debt reduction needed to reach the account's target based on its lowest applicable LTV and configured buffer. It requires usable pricing and backing and does not proceed while the borrower is in liquidation or when the borrower address is registered as an Underscore Earn vault.

This mechanism serves dual purposes: protecting borrowers through gradual deleveraging while helping maintain GREEN's $1 peg during market stress.

**Want to take action before redemption?** An owner can use the [specific-assets deleverage route](05-deleverage.md#self-deleveraging-with-specific-assets) before the redemption zone when the account, asset, pricing, and authorization checks pass.

## The Liquidation Process

When liquidation becomes necessary, Ripe can use up to two configured phases—Stability settlement followed by Dutch auctions—while pursuing the repayment target. Valid asset configuration is auction-only, Stability-then-auction, or neither; Stability settlement cannot be enabled without auction fallback. At runtime, a compatible funded Stability cohort can still complete the target before an auction begins.

AuctionHouse liquidation and ordinary credit redemption also skip a borrower address registered as an Underscore Earn vault. This exception follows the borrower address; it does not apply to an ordinary user merely because that user deposited an Earn-vault share as collateral.

### Tokenized Stock Collateral Follows Configured Paths

A tokenized stock does not receive a redemption or liquidation route merely because it belongs to that asset category. Asset and deployment configuration independently determine which paths and recipient controls apply:

* **Before liquidation**: Direct redemption can transfer a Stock Token only when redemption is enabled for the position and asset and the recipient passes the applicable checks. Standard GREEN repayment and eligible [deleverage](05-deleverage.md) remain separate ways to reduce debt.
* **At the liquidation threshold**: Reaching the threshold makes the account eligible; it does not move the Stock Token automatically. An eligible caller must submit a liquidation transaction.
* **After submission**: Stability settlement applies only when enabled for the Stock Token and a compatible, sufficiently funded Stability cohort can accept it. Auction-only configuration skips that phase; Stability-enabled configuration retains auction fallback for any eligible remainder.
* **What can be lost**: A limited shortfall may consume only part of the eligible Stock Token collateral. A severe shortfall can exhaust all eligible Stock Tokens and other collateral without fully clearing the debt.

### Before Liquidation: Deleverage

Before your position reaches liquidation, you have options to reduce debt without liquidation penalties:

* **Repay GREEN** through the standard repayment path without consuming collateral
* **[Deleverage](05-deleverage.md)** with eligible sGREEN or stable-side collateral when its configured route is available
* Eligible sGREEN can be redeemed to GREEN and burned; configured stable-side collateral can be transferred to Endaoment and credited at its oracle-valued amount
* No liquidation fees, no discounts — just debt reduction

Standard repayment uses its own repayment path. For [Deleverage](05-deleverage.md), the owner or an authorized caller can use the specific-assets route without waiting for the redemption zone. An untrusted third party can use the broad route only once the account is in the redemption zone, subject to its health-restoration cap.

**Key Point**: Wallet GREEN uses standard repayment. Assets configured to burn as payment or transfer to Endaoment can be handled through deleveraging, while liquidation processes assets configured for Stability settlement and/or auctions.

### What Happens When Liquidation Starts

When an eligible liquidation call is submitted after your position reaches or crosses the liquidation threshold:

1. **Position enters `inLiquidation` state**
   * You are blocked from taking new borrows
   * All ordinary withdrawals are blocked, including zero-LTV assets
   * You can still repay debt to exit liquidation
   * Collateral enabled for a configured liquidation route becomes eligible for processing

2. **Liquidation fees are calculated once per episode**
   * The configured base liquidation fee and bounded keeper fee are assessed only when the account first enters liquidation
   * The combined base and keeper fees are capped by available collateral surplus; the keeper fee is reduced first, and both fees can fall to zero when surplus is insufficient
   * Stability-pool spread can cover at most the nominal base fee. Any unpaid base fee and the keeper fee are added to debt before repayment settlement; a productive keeper receives the keeper reward in GREEN or, optionally, sGREEN
   * A first pass that repays nothing and starts no auction is fee- and settlement-inert and earns no keeper fee, but it still places the account in `inLiquidation`

3. **Configured asset processing begins**
   * Eligible collateral is processed toward the calculated repayment target
   * Up to two configured phases can apply: compatible Stability settlement followed by auction fallback for any eligible remainder
   * You keep any collateral that remains, but severe shortfalls can exhaust eligible collateral

4. **Retrying or exiting liquidation mode**
   * An outstanding auction owns the current pass; another liquidation pass waits until no auction remains
   * A retry within the same episode does not assess the base or keeper fee again
   * Repay until debt is zero or no greater than current max-borrow capacity to exit `inLiquidation`; merely moving outside the liquidation threshold is not enough
   * Once debt health is restored, normal operations resume

### Phase 1: Stability Pool Swaps

The protocol engages [stability pools](../earning-and-rewards/02-stability-pools.md) only for liquidation assets whose configuration enables this route. A tokenized stock enters this phase only when a compatible, funded Stability cohort can accept it; its asset type does not select the route by itself.

**How Pool Swaps Work**

1. A configured collateral asset needs liquidation
2. A compatible [Stability pool](../earning-and-rewards/02-stability-pools.md) holds an eligible settlement asset such as [sGREEN](../earning-and-rewards/01-sgreen.md) or a supported LP position
3. The cohort receives collateral while supplying settlement value at the effective liquidation spread
4. Depositors participate proportionally through vault shares; active collateral claims enter cohort NAV at usable oracle value

Before collateral moves, the Stability vault must confirm that the settlement cohort can accept the liquidation asset and has capacity. If that check fails, auction-enabled collateral can continue to the Dutch-auction route.

**The Conditional Settlement Dynamic**

* Compatible, funded pool liquidity can settle collateral before an auction
* The effective spread determines settlement value supplied, while active collateral claims are valued through the configured oracle path for cohort NAV
* The route can reduce the amount that needs immediate external execution, but it does not guarantee freedom from market movement or slippage
* Any configured auction-eligible remainder can continue to the Dutch-auction route

**Additional Pool Benefits**

* Depositors can earn RIPE rewards when the applicable Stakers allocation and reward terms are configured
* GREEN holders can redeem against available pool collateral only when general and asset redemption are enabled and recipient eligibility, usable pricing, and claim-custody checks pass
* Flexible withdrawal lets depositors choose which assets to claim

**Optional Access and Recipient Controls**

Whitelists and recipient checks are independent asset configuration, not a property of every tokenized stock or other tokenized asset. When configured, the relevant participant and recipient must pass those checks during settlement. A whitelist does not by itself enable Stability settlement, and a tokenized stock does not inherently require a permissioned pool.

For Stock Tokens, passing Stability access and recipient checks does not establish eligibility under issuer terms; see [Stock Tokens as Collateral](03-collateral-assets.md#stock-tokens-as-collateral).

_For deeper understanding of stability pool mechanics, see_ [_Stability Pools_](../earning-and-rewards/02-stability-pools.md)_._

### Phase 2: Dutch Auctions

Dutch auctions process remaining auction-enabled collateral when configured pool routes do not complete the target. A tokenized stock reaches this phase only when its auction route is enabled:

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

For a Stock Token auction, passing protocol access and recipient checks likewise does not establish eligibility under issuer terms; see [Stock Tokens as Collateral](03-collateral-assets.md#stock-tokens-as-collateral).

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
* Total impact: The configured terms can create a collateral-acquisition opportunity, but they do not guarantee a participant profit

**How Settlement Value Moves**

1. **Stability Pool Depositors**: Participate through vault shares in cohort custody and active claims valued through the configured oracle path
2. **Keepers**: Can earn configured rewards for productive qualifying liquidations
3. **Auction Buyers**: Pay and burn GREEN for collateral at the auction's current configured discount
4. **Endaoment**: Receives eligible non-GREEN settlement assets transferred from a Stability pool; GREEN used for settlement is burned, including GREEN obtained by redeeming consumed sGREEN

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

## Ripe Liquidation Mechanics

### At a Glance

| Mechanic | What Ripe Does |
| -------- | -------------- |
| **Repayment target** | Calculates an amount intended to restore safer account health; a severe shortfall can produce a full-debt target and exhaust eligible collateral |
| **Before liquidation** | Keeps ordinary repayment, eligible deleverage, and conditional redemption as separate debt-reduction paths |
| **Stability settlement** | Uses this route only when enabled and a compatible, sufficiently funded cohort can accept the asset |
| **Auction fallback** | Sends auction-enabled remainder to a Dutch auction when that route is configured |
| **Execution** | Requires an eligible caller to submit a liquidation transaction |
| **Pricing and fees** | Applies configured spreads, auction discounts, and bounded liquidation and keeper fees through their respective mechanisms |

### Borrower Protection Features

1. **Proactive Deleveraging**: Reduce debt before liquidation without liquidation fees
2. **Graduated Intervention**: Redemption buffer before liquidation
3. **Targeted Liquidations**: Limited shortfalls may be partial; severe ones can produce a full-debt target and exhaust eligible collateral without fully clearing the debt
4. **Configured Discounts**: Stability spreads and time-varying auction discounts follow their respective protocol settings
5. **Multiple Mechanisms**: Up to two configured phases can process eligible collateral

### System Stability Benefits

1. **Conditional Pre-Auction Settlement**: Compatible, funded Stability pools can reduce the amount needing auction execution
2. **Auction Fallback**: Auction-enabled collateral can proceed to time-based price discovery when pool settlement does not complete the target
3. **Target-Based Processing**: Limited shortfalls may leave eligible collateral in the borrower's position
4. **Permissionless Execution**: Eligible callers can submit liquidation transactions

## What If Bad Debt Occurs?

Extreme market conditions can leave residual borrower debt after eligible collateral is exhausted. If governance separately records an amount as protocol bad debt, qualifying bond purchases provide one recovery path:

**Bond Sales for Recovery**: A qualifying [bond](../governance-and-economics/03-bonds.md) purchase can credit its oracle-valued payment against recorded bad debt. In this process:

* The complete computed RIPE payout must fit the available bond allowance
* RIPE attributed to cleared bad debt is accounted outside ordinary bond-distribution usage
* Authorized cap administration must count that payout toward RIPE's protocol-wide 1 billion-token cap across all blockchains
* The purchase remains subject to bond, pricing, capacity, and minting controls

Authorized issuance administration must count RIPE attributed to this recovery path toward the protocol-wide cap. The mechanism does not guarantee that every amount of bad debt will be cleared.

## Liquidations That Don't Ruin Lives

Here's what actually matters:

**When liquidation occurs on Ripe**: A limited shortfall may leave collateral and an active position; severe undercollateralization can consume all eligible collateral without fully clearing the debt.

Even better: you can repay GREEN through the standard repayment path or [deleverage](05-deleverage.md) with eligible sGREEN or stable-side collateral before liquidation, potentially preserving tokenized-stock collateral without liquidation penalties.

The protocol doesn't do this to be nice. It does it because borrowers who survive keep borrowing, keep paying interest, keep the system running. Your success is the protocol's success.

So go ahead. Take that loan. Standard repayment, deleverage, redemption, Stability settlement, and auctions can protect the position when their respective eligibility and configuration checks pass.

***

_For technical implementation details, see the_ [_AuctionHouse Technical Documentation_](https://ripe-finance.gitbook.io/ripe-developers/core-lending/auctionhouse)_._
