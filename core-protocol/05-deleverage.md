---
description: Proactive Debt Reduction Without Liquidation Penalties
---

# Deleverage: Reduce Risk Before It's Too Late

Getting liquidated sucks. The fees, the forced selling, the stress.

But what if you could reduce your debt before things get ugly? That's what deleveraging does. Use eligible deposited assets, pay down debt, stay in control. No liquidation penalties. No keeper fees. Just a rational way to manage risk.

Even better: configured deleverage routes can consume eligible assets already deposited in Ripe. An eligible sGREEN Stability position can be redeemed to GREEN and burned, while supported stable-side collateral can be transferred for debt credit. Paying GREEN from a wallet is standard repayment instead; GREEN is not an ordinary Stability deposit.

## Quick Overview

**What is Deleverage?**

Deleveraging is the voluntary (or protocol-assisted) use of eligible deposited assets to reduce debt. Unlike liquidation, it can happen before you reach the danger zone and carries no liquidation penalties.

**Key Differences:**

| Aspect | Deleverage | Liquidation |
|--------|------------|-------------|
| When it happens | When authorized and eligible; untrusted broad deleverage requires the redemption zone | At or below the liquidation threshold after an eligible submitted call |
| Who triggers it | You, delegated addresses, or third parties (when eligible) | Anyone (keepers) |
| Penalties | None | Configured liquidation + keeper fees |
| Your control | Specific-assets route lets an authorized caller choose | Protocol-controlled settlement through configured Stability and/or auction routes |

## When Can You Be Deleveraged?

### Self-Deleveraging with Specific Assets

Through the specific-assets route, an account owner can choose the eligible configured asset order and target repayment without first entering the redemption zone. An approved protocol caller or a `canBorrow` delegate can use the same route for the owner when authorized. In each case, the protocol must be able to value and process the selected assets:

* Choose which eligible configured assets to use and in what order
* Specify the target amount of debt to repay
* No redemption-zone threshold requirement
* The owner needs no delegated permission; other callers need the route's authorization

### Third-Party Deleveraging (Redemption Zone)

When your position enters the **redemption zone**, others can help deleverage you:

* Triggered when collateral value is at or below the redemption threshold
* Third parties can initiate deleverage to restore your position health
* Limited to the amount needed to return you to a safe LTV
* Still no liquidation penalties applied

Broad deleverage evaluates authorization separately for each account; authority over one account does not carry over to another account in the same batch. The broad route has different authorization from the specific-assets route: an ordinary owner self-call is not automatically trusted, and an untrusted call proceeds only when the account is in the redemption zone and remains capped by the health-restoration calculation.

## How Deleveraging Works

When deleveraging occurs, assets are processed in priority order:

**Phase 1: Eligible Stability Positions**

Eligible positions in Stability vaults are considered first when their configured route is available:

* **sGREEN positions**: Eligible sGREEN can be redeemed to GREEN, then burned to reduce debt
* **Other configured Stability assets**: Used only when their asset configuration supplies an eligible deleverage route
* **Wallet GREEN**: Repaid through the standard repayment flow, not treated as a Stability deposit or collateral-based deleverage asset

Unavailable Stability liquidity is skipped instead of blocking the broader deleverage route.

**Later Phases: Other Configured Payment Assets**

After eligible Stability positions, the broad route considers configured priority assets and then the user's remaining vaults. An asset reduces debt only when its configuration supplies a burn-as-payment or transfer-to-Endaoment route:

* Configured stable-side assets—for example, supported stablecoins—transferred to Endaoment at their credited value
* No liquidation discount — debt reduction follows the credited oracle value, subject to transfer and rounding behavior
* Assets without either ordinary deleverage route are skipped; arbitrary volatile collateral requires a separately authorized volatile-asset route

### Choosing Specific Assets

You can specify exactly which assets to deleverage:

The owner, an approved protocol caller, or a `canBorrow` delegate can choose the asset order. Underscore registration alone does not authorize another user's account.

```
Hypothetical example: preserve a Stock Token with specific assets

You have:
- Stock Token A ($15,000)
- Eligible stable-side collateral ($10,000)
- An eligible sGREEN Stability position ($1,000)

Debt: $12,000

You choose to deleverage with:
1. sGREEN first ($1,000 debt reduction)
2. The stable-side asset second ($10,000 debt reduction)
3. Preserve Stock Token A

Result: $1,000 debt remaining and Stock Token A preserved
```

That outcome depends on the selected assets and routes being eligible and the account holding enough of those assets. If you instead repay with GREEN from your wallet, standard repayment can reduce the debt without consuming deposited collateral; that is a separate path.

### Full-Payoff Buffers and Dust

A trusted full-payoff route, including an owner-authorized specific-asset route, can use configured settlement extras when the position owner is not a registered Underscore Earn vault:

* A full-payoff buffer can authorize a bounded amount of collateral value above the live debt. The buffer is capped by both an absolute amount and a debt-relative percentage.
* Debt credit never exceeds the live debt. If the route consumes collateral above that amount, the overage receives no additional debt credit.
* If consumed collateral falls just short of the debt, the route can forgive the remainder only when it fits both configured dust caps. That forgiven remainder clears debt without an equivalent GREEN burn.
* Registered Underscore Earn-vault owners are excluded from these full-payoff extras, and an ordinary untrusted deleverage does not receive them.

These rules apply only when the corresponding configuration is nonzero. They are separate from liquidation fees: deleverage still does not charge a liquidation or keeper fee.

## Using [Underscore](https://underscore.finance/) Vaults?

An Underscore vault can manage its own Ripe position through authorized strategy calls. Its borrowing, deleverage, and withdrawal-adjustment behavior depends on that vault's implementation and configuration; using one does not eliminate the need to understand those mechanics.

For details on how Underscore vaults interact with Ripe, see [Underscore Protocol Integration](02-borrowing.md#underscore-earn-vault-integration).

## Delegation for Deleveraging

You can authorize others to manage your position:

### Available Permissions

* **canBorrow**: Allows delegate to deleverage on your behalf (uses same permission as borrowing)
* Trusted addresses can specify asset order and amounts; in a broad multi-user call, authority is evaluated separately for each user
* Protocol contracts (like Underscore) can deleverage for operational needs

### Setting Up Delegation

Delegated addresses can:

* Initiate deleverage when your position is at risk
* Choose which eligible configured assets to use and in what order
* Maintain your position health automatically

## When to Deleverage

Don't wait for liquidation. Consider deleveraging when:

* Market volatility increases
* Your LTV approaches the warning zone
* You want to reduce exposure to specific assets
* You're rotating into different collateral

## How Deleverage Fits In

Deleverage is one of several protective mechanisms, each activating at different risk levels:

```
Healthy / Warning: Owner or authorized caller may use the specific-assets route
Redemption Zone:   Specific-assets route + eligible broad deleverage or redemption
Liquidation Zone:  Eligible liquidation; each configured route keeps its own checks
```

* **Redemption**: GREEN is treated as a $1 debt-value input when eligible collateral is sized and credited (no liquidation penalty)
* **Deleverage**: Your assets reduce your debt without liquidation penalties; the specific-assets route lets an authorized caller choose the order
* **Liquidation**: Forced sale with configured liquidation and keeper fees (last resort)

An owner using the specific-assets route can self-deleverage before entering the redemption zone. The broad multi-user route evaluates authority separately for each account; an ordinary owner self-call follows the redemption-zone cap unless the caller is a trusted protocol or registered Underscore address. Immediately before settlement, the protocol re-reads the account's debt; if collateral interaction changed that debt, the entire deleverage transaction reverts. For the complete picture, see [Liquidations](04-liquidations.md).

## Taking Control of Your Risk

Liquidation is a worst-case scenario. Deleverage is a tool.

Smart borrowers:

1. Monitor their position health
2. Set up delegation for automated protection
3. Keep eligible sGREEN or stable-side collateral available for configured debt-reduction routes
4. Deleverage proactively rather than reactively

Deleverage does not apply liquidation or keeper fees. Debt credit, full-payoff buffers, asset-transfer behavior, and transaction costs still depend on the route and its configuration.

Stay ahead of the liquidation threshold. Stay in control.

***

_For technical implementation details, see the_ [_Deleverage Technical Documentation_](https://ripe-finance.gitbook.io/ripe-developers/core-lending/deleverage)_._
