---
description: Acquire RIPE through configurable vesting
---

# RIPE Reserve Engine: Acquire RIPE with Vesting

The RIPE Reserve Engine accepts a configured payment token and creates a vesting position for newly issued RIPE. Payment is collected when the position is created, while RIPE is minted only as vested amounts are claimed.

This is a separate distribution mechanism from [Ripe Bonds](03-bonds.md). A bond uses BondRoom epoch pricing, can add lock and activity-booster bonuses, and can credit its payment against bad debt. A Reserve Engine acquisition uses its own epoch controller, vesting-duration bonus, and allocation budget; its payment goes to EndaomentFunds as a treasury asset and does not use BondRoom's bad-debt accounting.

All payment assets, rates, capacities, durations, and availability controls described here are configurable. Examples explain the mechanism rather than asserting that it is deployed or enabled on a particular network. Use [RIPE Params](https://params.ripe.finance) for the current network configuration it exposes; do not infer availability from this guide.

## Acquisition Flow

A user first previews an acquisition using a payment amount and requested vesting length. The quote identifies:

* The payment amount and selected vesting length
* Base RIPE and any vesting-duration bonus
* The current epoch and payout-rate information
* The position's creation, first-claim, and maturity blocks

Execution binds the requested vesting length and epoch, a minimum RIPE output, and a deadline. The actual schedule is anchored to the transaction's inclusion block, so previewed schedule blocks move if execution is delayed.

Acquisitions are full-fill only. The Engine verifies exact payment-token receipt before creating the allocation; invalid payment, stale quote, or insufficient capacity reverts instead of creating a partial position.

On success:

1. The payment is sent to EndaomentFunds.
2. The full base-plus-bonus allocation is reserved from the Vesting contract's remaining allocation budget.
3. A position with a stable identifier and vesting schedule is recorded for the beneficiary.
4. No RIPE is minted yet.

## Epoch Pricing and Capacity

Each committed epoch snapshots the terms used for acquisitions in that epoch, including its payment capacity, minimum payment, vesting range, and base payout rate. Later configuration changes do not rewrite an already committed epoch.

The controller can adjust the next epoch's base rate from the preceding committed epoch's utilization and timing data, subject to configured bounds and idle-decay rules. The rate therefore stays fixed within a committed epoch while the controller can respond between epochs.

The vesting-duration bonus is separate from the base rate. A longer selected duration can increase the total RIPE allocation according to the configured bonus curve. Governance also sets an all-in payout-rate ceiling so the base rate plus the maximum duration bonus remains bounded.

## Allocation Budget

The Vesting contract tracks a dedicated `remainingAllocationBudget`. A new position must fit its full base-plus-bonus allocation, and creating the position reduces that budget immediately.

Claims do not replenish the allocation budget. This separates three ideas:

* **Epoch payment capacity** limits accepted payment within an epoch.
* **Vesting allocation budget** limits the total RIPE that new Reserve Engine positions can reserve.
* **Claimable amount** is the vested portion of an already reserved position.

The budget is an onchain accounting allowance, not pre-minted RIPE held in escrow. Claiming still depends on the Engine's RIPE mint authorization and the protocol minting circuit breaker.

## Catch-Up Cliff and Linear Vesting

Every position stores a creation block, claim-start block, and maturity block. The epoch's snapshotted minimum vesting length establishes the claim cliff:

```text
claim start = creation block + minimum vesting length
maturity    = creation block + selected vesting length
```

Nothing is claimable before the claim-start block. At that block, all RIPE that has vested linearly since creation becomes claimable at once. Vesting then continues linearly from creation until maturity. This is a **catch-up cliff**, not a schedule that begins vesting only after the cliff.

**Illustrative example:** Assume a position has a 12-month selected duration and a 3-month minimum duration. Nothing can be claimed during the first 3 months. At the claim start, approximately one quarter of the allocation has accrued under the linear schedule and becomes claimable; the remainder continues vesting through month 12. These durations are example inputs, not live terms.

If the selected and minimum durations are equal, the entire allocation becomes claimable at the cliff, which is also the maturity block.

## Claims and Optional Governance Deposit

A beneficiary can claim vested RIPE from a position. RIPE is minted only after the Vesting contract records a valid, nonzero claim.

* **Direct claim:** RIPE is minted to the beneficiary.
* **Auto-deposit claim:** RIPE is minted through the Engine and deposited for the beneficiary into the core RipeGov vault currently selected in Mission Control.

For auto-deposit, the submitted lock duration is a request. The current core RipeGov vault applies its configured minimum, maximum, and whole-position lock mechanics; a separate position in a historical governance vault is not merged automatically.

## Lifecycle and Governance Controls

The Engine and Vesting contracts have separate pause boundaries:

* Pausing the Engine stops new acquisitions but does not stop claims.
* Pausing Vesting stops claims and also makes the Engine reject new acquisitions.
* RIPE token controls, mint authorization, and the protocol minting circuit breaker can block claim settlement and new acquisitions.

Governance can start, stop, pause, and configure the mechanism through its authorized paths. Exact terms and control settings are deployment configuration; this guide does not establish availability.

## What to Check Before Acquiring

The preview is a quote, not a guarantee that a later transaction will succeed. Before execution, verify:

* The configured payment token and your allowance
* The quoted epoch, vesting duration, RIPE output, and deadline
* Remaining epoch capacity and Vesting allocation budget
* The claim-start and maturity schedule
* Whether you want direct RIPE delivery or an optional RipeGov deposit when claiming

The accepted payment becomes a protocol treasury asset immediately, while RIPE arrives only as the position vests and claims succeed. Treat the vesting schedule, token controls, mint authorization, and smart-contract risk as part of the acquisition.
