---
description: Acquire RIPE while adding protocol treasury assets
---

# Ripe Bonds: Acquire RIPE for Treasury Assets

Every protocol needs capital. Most just sell tokens and spend it.

Ripe bonds exchange a configured payment asset for RIPE and send the accepted payment to the [Endaoment](../core-protocol/07-endaoment.md). Choose a qualifying lock? A configured bonus can increase your allocation. Have an eligible activity booster? That can add another bonus.

Bonding is a configured route for adding protocol-owned treasury assets while distributing RIPE under explicit epoch and budget controls.

The [RIPE Reserve Engine](04-reserve-engine.md) is a separate acquisition path: it collects payment at position creation and mints RIPE only as the resulting allocation vests and is claimed. BondRoom instead settles a successful bond purchase immediately, either as unlocked RIPE or through a qualifying RipeGov deposit.

## The Bond Value Proposition

### Why Bonds Matter

Bonds solve a critical challenge in DeFi: how to bootstrap protocol-owned liquidity without relying on mercenary capital or unsustainable incentives. Through bonds, Ripe Protocol:

* **Accumulates protocol-owned treasury assets** that can be retained or deployed through authorized actions
* **Can support configured liquidity operations** without relying solely on rented liquidity
* **Distributes RIPE by explicit rules** based on accepted whole payment-token units, epoch progress, and any qualifying bonuses
* **Aligns incentives** between token holders and protocol health

A successful bond purchase sends the accepted payment to EndaomentFunds and delivers the calculated RIPE payout under the bond's checks.

## Bond Mechanics: How It Works

### Epoch-Based Distribution

Bonds operate through time-limited epochs that bound token distribution:

* **Payment Capacity Per Epoch**: Each epoch accepts up to a configured amount of the payment asset, subject to the remaining RIPE bond allowance
* **Time Windows**: Epoch length is configurable in protocol blocks
* **Shared Access**: Purchases consume the remaining payment capacity in transaction order
* **Auto-Renewal**: Configuration determines whether an exhausted epoch schedules another epoch and the delay before it starts
* **Availability**: Bonding may be paused during certain protocol phases and enabled by governance

The epoch bounds how much payment can be accepted during a window; it does not guarantee capacity to every participant.

### Time-Based Epoch Pricing

Within each epoch, RIPE prices follow a descending curve:

```
Start of Epoch → Higher Price → Less RIPE per Payment Unit
                      ↓
                 Time Passes
                      ↓
End of Epoch → Lower Price → More RIPE per Payment Unit
```

With the epoch's pricing inputs unchanged, the onchain curve increases the RIPE paid per whole payment unit as the epoch progresses:

* **Earlier buyers** may encounter more remaining epoch and RIPE-budget capacity, but execution is never guaranteed
* **Later blocks** produce a higher RIPE-per-unit result under the same configured curve, but may have less capacity remaining
* **Current-state calculation** is inspectable onchain; governance changes, transaction ordering, capacity, and slippage checks can still change or reject execution

### Token Allocation

The bonding program draws against the Ledger's `ripeAvailForBonds` accounting allowance. This is not an escrowed balance of RIPE tokens. The allowance is separate from the payment capacity available in an epoch: a purchase must fit both before it can complete.

RIPE is minted only as part of a successful purchase, either to the recipient or temporarily to BondRoom before deposit into the governance vault. That final mint-and-delivery path remains subject to RipeHq mint authorization and its minting circuit breaker, RIPE token controls, and any required governance-vault deposit succeeding.

## Maximizing Your Bond Value

### Base Bond Rate

Every bond starts with a base exchange rate determined by:

* **Current epoch progress** (0-100%)
* **Price range** set by governance
* **Configured payment asset**

The smart contract calculates the RIPE allocation from the state used by that transaction. A preview is informative, but it does not reserve capacity or prevent intervening state or configuration changes.

### Payment Amount and Minimum RIPE Payout

A purchase specifies a maximum payment amount and may specify a minimum acceptable RIPE payout. The protocol caps the payment by the caller's available balance and the epoch's remaining capacity, then charges only **whole payment-token units** based on that token's decimals.

Any fractional remainder or unfilled transferred amount is refunded to the transaction caller, even when the bond recipient is another address. After the total payout is calculated, the Teller checks the minimum RIPE payout. If the result is too low, the entire transaction reverts, including the payment transfer.

**Hypothetical example**: If a two-decimal payment token is configured to pay 4 RIPE per whole unit and a user submits 12.75 units, the bond uses 12 whole units, charges 12.00, refunds 0.75, and calculates 48 RIPE before lock or booster bonuses. These are illustrative values.

### Lock Duration Bonuses

Committing to lock your RIPE can increase your bond value through a configured percentage-based bonus. The scale below is a hypothetical example, not live lock terms:

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

* The requested duration is capped at the configured maximum
* If the resulting duration is below the configured minimum, the contract resets it to zero: the RIPE is delivered unlocked and receives no lock bonus
* A booster that produces a nonzero bonus can raise the duration to its own minimum before the bond's minimum-duration check, still capped by the bond maximum
* Bonus calculated as percentage of base bond amount
* Locked RIPE automatically deposits into the current core [Ripe Governance Vault](02-governance.md) resolved through Mission Control
* Start accumulating governance points and any configured staking rewards after deposit
* Keep the bond position in the governance vault for the selected lock period

### Bond Boosters

The protocol rewards specific user activities through Bond Boosters, creating targeted incentives for valuable contributions:

**How Bond Boosters Work:**

* **Automatic Application**: A valid Bond Booster is checked automatically during each bond purchase
* **Whole-Purchase Capacity**: The purchase receives the booster only if all of its whole payment-token units fit within the user's remaining allowance
* **Minimum Lock Required**: A booster can impose a configured minimum lock duration
* **Time Windows**: A booster can expire at a configured block
* **Percentage Multipliers**: The configured boost ratio adds a bonus to the base bond amount

**Illustrative Bond Booster Program:**

The program and values below illustrate how a booster can be configured; they are not live protocol parameters.

* **Ripe Radness**: Rewards testnet participants
  * Bond Booster range: 10% to 200% based on contribution level
  * Top contributors receive maximum 200% Bond Booster
  * Most participants receive between 10-100% Bond Booster
  * Limited units per participant
  * Expires at predetermined block
  * Eligibility verified through Discord roles

**The Unit System:** Units represent your Bond Booster capacity:

* In this example, 1 unit = 1 USDC
* A qualifying boosted purchase consumes all of its units against the current booster grant
* Removing a grant resets its recorded usage. Installing a grant resets usage only when the previous grant is absent or expired; replacing a still-live grant preserves the units already used
* If the entire purchase does not fit within the remaining units, that purchase gets no booster and consumes no units

Example scenarios with 1,000 units and 200% Bond Booster:

* **Scenario A**: Bond 500 USDC → uses 500 units, all 500 USDC gets boosted (500 units remain)
* **Scenario B**: Bond 1,500 USDC → the purchase exceeds the 1,000-unit allowance, so none of it is boosted and no units are consumed
* **Scenario C**: Bond 500 USDC first, then bond 1,000 USDC later → the first purchase uses 500 units; the second exceeds the 500 units remaining, so the second purchase gets no booster and consumes no additional units

To use a booster, keep each qualifying purchase within the units still available to that user.

**Stacking Example:** Top-tier testnet participant bonds 1,000 USDC with 3-year lock:

* Base: 2,000 RIPE (at $0.50/RIPE)
* Lock bonus: +4,000 RIPE (200% of base)
* Radness Bond Booster: +4,000 RIPE (200% of base for top contributors)
* **Total: 10,000 RIPE (5x multiplier)**

_Note: Most participants receive smaller Bond Boosters. With a 50% Radness Bond Booster, total would be 7,000 RIPE (3.5x multiplier)_

## Hypothetical Bonding Examples

Every value in the examples below is illustrative.

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

When protocol liquidations create bad debt, bonds serve as a recovery mechanism:

**How Bond Purchases Work During Bad Debt:**

* A purchase must satisfy the bond's enablement, payment-asset, epoch-capacity, and payout-availability checks
* The complete calculated RIPE payout—including lock and booster bonuses—must fit the available bond allowance before the purchase can proceed
* The oracle-valued payment can reduce recorded bad debt up to the outstanding amount

**Supply Expansion Mechanism:**

* After the full-payout availability check passes, RIPE attributed to the bad-debt portion is recorded outside ordinary bond-distribution usage
* This accounting can expand minted supply beyond the base token allocation
* This expansion dilutes all RIPE holders proportionally
* Protocol transparently tracks all additional RIPE minted

**Design Rationale:**

* A successful purchase receives its complete computed payout; a purchase that fails a protocol or availability check reverts
* Any supply expansion associated with bad-debt recovery dilutes RIPE holders
* Bond proceeds still become Endaoment treasury assets while the oracle-valued payment is credited against bad debt
* Governance has incentive to minimize bad debt to prevent dilution

This lets qualifying bond purchases serve as both a treasury-building tool and a bad-debt recovery path without bypassing the bond's normal preconditions.

## Bond Proceeds and Treasury Management

Accepted payment assets from bond sales flow directly to the [Endaoment](../core-protocol/07-endaoment.md), becoming protocol treasury assets:

**Treasury Deployment Strategy:**

* **Yield Positions**: Can enter compatible [Underscore](https://underscore.finance/) integrations when configured
* **GREEN Liquidity**: Can provide trading depth through authorized actions
* **Market Operations**: Can support GREEN's peg through bounded transactions
* **Strategic Reserves**: Can remain in treasury custody until an authorized use

**The Enforced Flow:**

```
Accepted Bond Payment → EndaomentFunds Custody
                                  ↓
             Retained Reserve or Authorized Operation
```

Accepted bond payments become protocol treasury assets. They may be deployed through configured integrations, used in authorized market operations, or retained in reserve.

## Time to Choose: Mercenary or Builder?

Here's the choice that matters:

Bond with a qualifying lock? Receive the configured lock bonus. Add an eligible Bond Booster? Receive its configured bonus too.

Wait for exchanges? Pay market price. No bonuses. No multipliers. Just hoping someone sells.

But this isn't only about the discount. The accepted payment becomes protocol-owned Endaoment treasury capital. It can remain in custody or be used through authorized, configured operations; bonding does not guarantee yield or make each payment direct collateral backing for GREEN.

The protocol needs capital. You want tokens. Bonds make it happen.

Check the protocol interface for current bonding availability — epochs may be paused at times and enabled by governance as needed.

***

_Check current epoch status and calculate your potential RIPE allocation in the Ripe Protocol interface._
