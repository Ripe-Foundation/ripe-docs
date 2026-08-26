---
description: Trade Cash for Power
---

# Ripe Bonds: Trade Cash for Power

Every protocol needs capital. Most just sell tokens and spend it.

Ripe bonds exchange a configured payment asset for newly minted RIPE and route the accepted payment to [EndaomentFunds](../core-protocol/07-endaoment.md). Qualifying lock and booster bonuses are each calculated from the same base payout and added separately.

The longer everyone else waits, the more you accumulate.

Accepted payment funds protocol treasury custody, while the purchaser receives the computed RIPE allocation.

> **Examples, not live terms:** All assets, rates, capacities, durations, percentages, tables, and scenarios on this page are illustrative. See [RIPE Params](https://params.ripe.finance) for current onchain configuration.

The [RIPE Reserve Engine](04-reserve-engine.md) is a separate acquisition path: it collects payment when a vesting position is created and mints RIPE only as that allocation vests and is claimed. BondRoom settles a successful purchase immediately as unlocked RIPE or through a qualifying governance-vault deposit.

## The Bond Value Proposition

### Why Bonds Matter

Bonds provide a configured route for exchanging a payment asset for RIPE while building treasury custody. Through bonds, Ripe Protocol:

* **Accumulates treasury assets** in EndaomentFunds
* **Makes treasury assets available** for later authorized yield, liquidity, market, or reserve operations
* **Distributes RIPE fairly** based on actual capital contribution
* **Aligns incentives** between token holders and protocol health

Every successful purchase transfers the accepted payment to EndaomentFunds and mints the calculated RIPE payout. What the treasury does with that payment afterward requires separate authorization.

## Bond Mechanics: How It Works

### Epoch-Based Distribution

Bonds operate through time-limited epochs that ensure sustainable token distribution:

* **Payment Capacity Per Epoch**: Each epoch limits how much of the configured payment asset can be accepted
* **Time Windows**: Governance configures each epoch's length in blocks
* **Fair Access**: First-come, first-served within each epoch
* **Auto-Renewal**: New epochs can start automatically after sell-out
* **Availability**: Bonding may be paused during certain protocol phases and enabled by governance

An epoch can still sell out, and each purchase remains subject to remaining payment capacity, bond allowance, permission checks, pauses, and the configured window.

### Dynamic Pricing That Rewards Action

Within each epoch, RIPE prices follow a descending curve:

```
Start of Epoch → Higher Price → Less RIPE per Dollar
                      ↓
                 Time Passes
                      ↓
End of Epoch → Lower Price → More RIPE per Dollar
```

This creates interesting dynamics:

* **Earlier buyers** may encounter more remaining capacity, but every purchase still depends on the live checks above
* **Patient buyers** receive better prices
* **Market forces** determine actual demand
* **Transparent pricing** visible to all participants

### Token Allocation

Bond distribution uses a bond-specific accounting allowance, separate from rewards and Reserve Engine allowances. It is not escrowed RIPE or an independent supply cap; a successful purchase mints through the authorized bond path.

## Maximizing Your Bond Value

### Base Bond Rate

Every bond starts with a base exchange rate determined by:

* **Current epoch progress** (0-100%)
* **Price range** set by governance
* **Configured payment asset and token decimals**

The smart contract calculates your exact RIPE allocation based on these parameters, ensuring transparent and predictable pricing.

### Payment Amount and Minimum RIPE Payout

A purchase specifies a maximum payment amount and may specify a minimum RIPE payout. Accepted payment is capped by available balance and remaining epoch capacity. The base RIPE payout counts only whole configured payment-token units (`10 ** token decimals`); a fractional portion does not increase that payout, while payment above the accepted cap is refunded to the transaction caller.

After the complete payout is calculated, the Teller checks the minimum RIPE payout. If it is too low, the entire transaction—including the payment transfer—reverts.

### Lock Duration Bonuses

Committing to lock your RIPE dramatically increases your bond value through percentage-based bonuses:

**The Lock Bonus Scale:**

```
No Lock → 0% bonus → 1x RIPE (base amount only)
3 Months → ~15% bonus → 1.15x total RIPE
6 Months → ~35% bonus → 1.35x total RIPE
1 Year → ~65% bonus → 1.65x total RIPE
2 Years → ~130% bonus → 2.3x total RIPE
3 Years → 200% bonus → 3x total RIPE
```

**How It Works:**

* The requested duration is capped at the configured maximum. A successful booster can raise it to the booster minimum; if the final duration remains below the bond minimum, it resets to zero and receives no lock bonus
* Bonus calculated as percentage of base bond amount
* Locked RIPE automatically deposits into the current core [Ripe Governance Vault](02-governance.md) resolved through Mission Control
* The resulting position follows that vault's point and reward eligibility rules

### Bond Boosters

The protocol rewards specific user activities through Bond Boosters, creating targeted incentives for valuable contributions:

**How Bond Boosters Work:**

* **Automatic Application**: Bond Boosters activate automatically during bond purchases
* **Unit-Based Limits**: Each user grant has a configured maximum number of whole payment-token units
* **Minimum Lock Effect**: A successful booster can raise the bond lock to its configured minimum, capped by the bond maximum
* **Time Windows**: Bond Boosters expire at specific block numbers
* **Additive Bonus**: The booster bonus is calculated from the base RIPE payout and added separately from the lock bonus

**Example Bond Booster Program:**

* **Ripe Radness**: Rewards testnet participants
  * Bond Booster range: 10% to 200% based on contribution level
  * Top contributors receive maximum 200% Bond Booster
  * Most participants receive between 10-100% Bond Booster
  * Limited units per participant
  * Expires at predetermined block
  * Eligibility verified through Discord roles

**The Unit System:** Units represent your Bond Booster capacity:

* One unit is one whole token of the configured payment asset
* A boost applies only when the complete purchase fits the user's remaining units; an oversized purchase receives no partial boost and consumes no units
* Removing a grant resets usage; installing an absent or expired grant resets usage, while replacing a still-live grant preserves usage

Example scenarios with 1,000 units and 200% Bond Booster:

* **Scenario A**: Bond 500 USDC → uses 500 units, all 500 USDC gets boosted (500 units remain)
* **Scenario B**: Bond 1,500 USDC → exceeds the 1,000-unit capacity, so no booster applies and no units are consumed
* **Scenario C**: Bond 500 USDC first, then bond 1,000 USDC later → the second purchase exceeds the 500 remaining units, so it receives no booster

**Stacking Example:** Top-tier testnet participant bonds 1,000 USDC with 3-year lock:

* Base: 2,000 RIPE (at $0.50/RIPE)
* Lock bonus: +4,000 RIPE (200% of base)
* Radness Bond Booster: +4,000 RIPE (200% of base for top contributors)
* **Total: 10,000 RIPE (5x multiplier)**

_Note: Most participants receive smaller Bond Boosters. With a 50% Radness Bond Booster, total would be 7,000 RIPE (3.5x multiplier)_

## Real-World Bonding Examples

### Example 1: Maximum Value Strategy

_Sarah bonds 5,000 USDC with full commitment_

* **Epoch Status**: 7 hours into 24-hour epoch (30% complete)
* **Base Rate**: $0.40 per RIPE → 12,500 RIPE base
* **3-Year Lock**: +200% → 25,000 RIPE bonus
* **Radness Bond Booster**: +200% → 25,000 RIPE bonus
* **Total Received**: 62,500 RIPE (locked 3 years)
* **Effective Price**: $0.08 per RIPE

### Example 2: Balanced Approach

_James bonds 10,000 USDC with moderate lock_

* **Epoch Status**: 17 hours into 24-hour epoch (70% complete)
* **Base Rate**: $0.25 per RIPE → 40,000 RIPE base
* **6-Month Lock**: +35% → 14,000 RIPE bonus
* **No Bond Booster**: 0 RIPE bonus
* **Total Received**: 54,000 RIPE (locked 6 months)
* **Effective Price**: $0.185 per RIPE

## Understanding Bad Debt Mechanics

When governance has recorded protocol bad debt, bonds can serve as a recovery mechanism:

**How Bond Purchases Work During Bad Debt:**

* Bond purchasers receive **100% of calculated RIPE allocation**
* The accepted payment's USD value can reduce recorded bad debt before the payment asset moves to EndaomentFunds
* Recorded bad debt falls only when the accepted payment has a usable nonzero oracle value, and only up to the outstanding bad debt

**Supply-Cap Treatment:**

* The complete payout, including lock and booster bonuses, must fit the available bond allowance
* RIPE attributed to bad-debt repayment is recorded outside ordinary bond-distribution usage
* Authorized cap administration must count that payout toward RIPE's protocol-wide 1 billion-token cap across all blockchains

**Design Rationale:**

* A successful purchase receives its complete computed payout; a failed availability or protocol check reverts
* Bad-debt recovery uses capacity within the same protocol-wide RIPE cap
* Bad-debt recovery can consume RIPE distribution capacity
* Governance has incentive to minimize bad debt and preserve RIPE distribution capacity

This mechanism ensures bonds function as both a treasury building tool and an emergency recovery system.

## Bond Proceeds and Treasury Management

Accepted bond payment assets flow directly to [EndaomentFunds](../core-protocol/07-endaoment.md), where later use follows treasury operations:

**Potential Authorized Treasury Uses:**

* **Yield Integrations**: Deposit into a registered integration through a separately authorized action
* **GREEN Liquidity**: Add or remove liquidity through configured, authorized operations
* **Market Operations**: Run bounded GREEN stabilizer actions when authorized
* **Treasury Reserves**: Retain assets in EndaomentFunds until another permitted action uses them

**The Conditional Treasury Path:**

```
Accepted Payment → EndaomentFunds Custody → Separately Authorized Operations
                                                   ↓
                                      Results Depend on Execution
                                        and Market Conditions
```

BondRoom itself does not invest the payment asset, guarantee yield, provide permanent liquidity, or make it backing for GREEN. It transfers accepted payment into treasury custody; later results depend on separately authorized operations and market conditions.

## Time to Choose: Mercenary or Builder?

Here's the choice that matters:

Choose a qualifying lock? Its configured bonus adds to the base allocation. Have an eligible Bond Booster with enough remaining units? Its separate bonus adds from that same base.

Wait for exchanges? Pay market price. No bonuses. No multipliers. Just hoping someone sells.

But this isn't only about the discount. Accepted payment becomes protocol treasury custody, available for the authorized operations described above.

The protocol needs capital. You want tokens. Bonds make it happen.

Check the protocol interface for current bonding availability — epochs may be paused at times and enabled by governance as needed.

***

_Check current epoch status and calculate your potential RIPE allocation in the Ripe Protocol interface._

_For technical implementation details, see the_ [_BondRoom Technical Documentation_](https://ripe-finance.gitbook.io/ripe-developers/treasury-and-rewards/bondroom)_._
