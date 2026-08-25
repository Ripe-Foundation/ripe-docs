---
description: Proactive Debt Reduction Without Liquidation Penalties
---

# Deleverage: Reduce Risk Before It's Too Late

Getting liquidated sucks. The fees, the forced selling, the stress.

But what if you could reduce your debt before things get ugly? That's what deleveraging does. It consumes eligible collateral, credits its value against debt, and avoids liquidation and keeper fees.

Depending on an asset's configuration, deleverage can burn GREEN-side Stability assets or transfer eligible collateral to Endaoment. It does not sell every kind of collateral through one unrestricted user route.

## Quick Overview

**What is Deleverage?**

Deleveraging is a debt-reduction mechanism separate from liquidation and credit redemption. An owner or trusted caller can use it proactively; an untrusted caller can use only the bounded broad route when the account is in the configured deleverage zone. Deleverage has no liquidation fee, but only eligible assets can settle debt through its ordinary routes.

**Key Differences:**

| Aspect | Deleverage | Liquidation |
|--------|------------|-------------|
| When it happens | Proactively for trusted callers; in a bounded danger zone for untrusted callers | When an eligible caller submits after the liquidation threshold |
| Who triggers it | Owner, trusted protocol caller, `canBorrow` delegate, or an eligible untrusted caller | A keeper or other permitted caller |
| Fees | No liquidation or keeper fee | Episode-based liquidation and keeper fees |
| Asset order | Trusted callers may submit an eligible asset order; the broad route follows protocol priority | Protocol priority traversal, Stability eligibility, then auction fallback |
| Amount | Trusted route target; untrusted route capped by the health-restoring calculation | Health-restoring target that can reach full debt |

## When Can You Be Deleveraged?

### Self-Deleveraging (Trusted Route)

You can deleverage your own position while it has debt and eligible assets:

* Submit an ordered list of eligible assets
* Specify a per-asset repayment target; actual debt credit is bounded by eligible collateral, live debt, and route behavior
* No threshold requirements
* No permissions needed

### Untrusted Deleveraging (Bounded Danger Zone)

When your position enters the configured **deleverage zone**, an address without owner or delegate permission can invoke the broad route:

* Triggered when collateral value falls below redemption threshold
* The account must not already be in liquidation
* The caller cannot choose an arbitrary asset order
* Repayment is capped at the amount calculated to return the account toward its safe target
* The same route remains free of liquidation and keeper fees

## How Deleveraging Works

Ripe exposes several deleverage routes with different authority and traversal rules.

### Broad and Batch Deleverage

The broad route can process one or many users. For each user, it determines whether the caller is trusted independently; authority over one account does not grant authority over another account in the same batch.

Assets are traversed in this order:

**Phase 1: Stability Pool Assets**

Configured priority Stability-vault cohorts are considered first:

* GREEN-side assets configured to burn as payment are withdrawn and burned for debt credit
* Stability-vault availability is checked fail-soft; an unavailable cohort is skipped rather than blocking the whole broad route
* A Stability position's claimable NAV is not treated as if it were all immediately available in the original settlement asset

**Phase 2: Priority Deleverage Assets**

The protocol then visits configured priority assets outside the Stability cohorts:

* Assets configured to transfer to Endaoment are moved there for debt credit
* Assets without either the burn-as-payment or transfer-to-Endaoment flag are skipped by ordinary deleverage

**Phase 3: Remaining User Vaults**

If the target remains, the route traverses the user's other vaults in their stored order, applying the same eligibility rules and avoiding a second pass over assets already handled.

### Choosing Specific Assets

The owner, a valid Ripe protocol caller, or a delegate with `canBorrow` permission can submit an ordered list of specific assets and per-asset repayment targets. The route processes only assets eligible for ordinary stable-side deleverage; listing an unrelated volatile asset does not make it user-deleveragable.

```
Example: Deleverage with Specific Assets

You have:
- an eligible Stability asset
- eligible collateral configured for transfer to Endaoment
- unrelated volatile collateral

You choose:
1. the Stability asset first
2. the transfer-eligible collateral second

The specific route processes those eligible assets in the submitted order.
The unrelated volatile collateral is not processed by this route.
```

### Volatile-Asset Deleverage

Volatile collateral uses a separate privileged route restricted to valid Ripe protocol addresses or governance's switchboard. This route deliberately skips assets configured for ordinary burn or Endaoment transfer. A user-controlled specific order cannot be used to bypass that privilege boundary.

### Atomic Debt Settlement

Deleverage plans against a debt snapshot, interacts with collateral, then re-reads live debt immediately before settlement. If the debt amount changed during those interactions, the transaction reverts atomically. Debt credit is capped at the real debt even when a narrowly bounded full-payoff buffer is used for rounding.

## Using [Underscore](https://underscore.finance/) Vaults?

When a compatible Underscore vault uses Ripe inside its strategy, its authorized strategy logic can manage the vault's Ripe position, including eligible deleverage actions. That behavior belongs to the external strategy and its configured permissions; Ripe does not automatically manage every Underscore position.

For details on how Underscore vaults interact with Ripe, see [Underscore Protocol Integration](02-borrowing.md#underscore-earn-vault-integration).

## Delegation for Deleveraging

You can authorize others to manage your position:

### Available Permissions

* **Owner**: Can use the ordered-specific route for their own account
* **canBorrow**: Makes a delegate trusted for broad deleverage and allows that delegate to submit a specific eligible asset order
* **Valid Ripe addresses**: Can use trusted protocol routes; trust is still evaluated for each user in a multi-user batch
* **Untrusted addresses**: Can use only the bounded broad route when its zone and health checks pass

### Setting Up Delegation

Delegated addresses can:

* Initiate broad deleverage on your behalf
* Choose eligible assets and their order through the specific route
* Submit eligible deleverage transactions as part of an authorized management process

## When to Deleverage

Don't wait for liquidation. Consider deleveraging when:

* Market volatility increases
* Your LTV approaches the warning zone
* You want to reduce exposure to a specific **eligible** asset
* You're rotating into different collateral

## How Deleverage Fits In

Deleverage is one of several protective mechanisms, each activating at different risk levels:

```
Healthy Zone → Warning Zone → Deleverage/Redemption Zone → Liquidation Zone
     |              |                    |                      |
Trusted routes   Borrowing may      Bounded untrusted        Separate
remain usable     be limited         routes may open         liquidation
```

* **Redemption**: A separate route in which GREEN holders exchange GREEN for eligible collateral under its own rules
* **Deleverage**: Eligible assets reduce debt without liquidation fees; ordering depends on caller authority and route
* **Liquidation**: A separate forced-settlement episode using Stability eligibility and auction fallback

Trusted deleverage is available proactively while the account and assets pass their checks. A caller without permission must wait for the bounded deleverage zone and cannot act after the account is already in liquidation. For the complete picture, see [Liquidations](04-liquidations.md).

## Taking Control of Your Risk

Liquidation is a worst-case scenario. Deleverage is a tool.

Smart borrowers:

1. Monitor their position health
2. Set up delegation for automated protection
3. Understand which deposited assets are actually eligible for deleverage
4. Deleverage proactively rather than reactively

The protocol does not add liquidation or keeper fees to a successful deleverage. The collateral consumed and debt credited still depend on eligibility, prices, route limits, and available balances.

Stay ahead of the liquidation threshold. Stay in control.

***

_For technical implementation details, see the_ [_Deleverage Technical Documentation_](https://ripe-finance.gitbook.io/ripe-developers/core-lending/deleverage)_._
