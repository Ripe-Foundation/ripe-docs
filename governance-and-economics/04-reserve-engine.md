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

Execution includes the quoted vesting length and epoch, a minimum RIPE output, and a deadline. Those are the bound execution checks. The previewed creation, claim-start, and maturity blocks are projections from the preview block; the actual position is anchored to the transaction's inclusion block, so those schedule blocks move if execution is delayed.

Acquisitions are full-fill only. The Engine verifies the exact increase in payment-token custody before it creates the RIPE allocation. A short receipt, false token return, fee-on-transfer result, expired deadline, stale epoch expectation, or insufficient capacity causes the transaction to revert instead of creating a partial position.

On success:

1. The payment is sent to EndaomentFunds.
2. The full base-plus-bonus allocation is reserved from the Vesting contract's remaining allocation budget.
3. A position with a stable identifier and vesting schedule is recorded for the beneficiary.
4. No RIPE is minted yet.

## Epoch Pricing and Capacity

Each committed epoch snapshots the terms used for acquisitions in that epoch, including its payment capacity, minimum payment, vesting range, and base payout rate. Later configuration changes do not rewrite an already committed epoch.

The controller can adjust the next epoch's base rate from the preceding committed epoch's utilization and timing data, subject to configured bounds and idle-decay rules. The rate therefore stays fixed within a committed epoch while the controller can respond between epochs.

The vesting-duration bonus is separate from the base rate. A longer selected duration can increase the total RIPE allocation according to the configured bonus curve. Governance also sets an all-in payout-rate ceiling so the base rate plus the maximum duration bonus remains bounded.

When the minimum and maximum vesting lengths differ, configuration validation also bounds the bonus relative to that duration range. The maximum-duration option cannot have an equal or faster average RIPE release rate solely because of its bonus.

An optional one-shot override can set the base rate for one resolved epoch. It is applied and consumed if a successful acquisition first commits that target epoch. If no acquisition commits the target before the clock advances, the next committed epoch clears the override as missed. The overridden rate becomes the historical starting point for the following controller transition rather than snapping back immediately. Engine start, stop, and controller changes invalidate an installed override; a pause or acquisition-availability change by itself does not.

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

A beneficiary can claim one position or submit a contract-bounded batch of position IDs. RIPE is minted only after the Vesting contract records a valid, nonzero claim.

* **Direct claim:** RIPE is minted to the beneficiary.
* **Auto-deposit claim:** RIPE is minted through the Engine and deposited for the beneficiary into the core RipeGov vault currently selected in Mission Control.

For auto-deposit, the submitted lock duration is a request. The live RipeGov vault applies its configured minimum, maximum, and whole-position lock mechanics. The deposit affects that RIPE asset position in the current core vault; a separate position in a historical governance vault is not merged automatically.

A batch is atomic. Callers should remove duplicate position IDs before submission because a duplicate that reaches the same position twice causes the entire batch to revert. A downstream mint, token, blacklist, or governance-vault failure also rolls back the complete claim transaction.

A blacklisted beneficiary's reserved allocation is not forfeited or returned to the budget. It remains outstanding until it can be claimed and therefore continues to prevent the Vesting contract from being retired. This contract version has no built-in position reassignment, migration, forfeiture, or liability-recovery route.

## Lifecycle and Governance Controls

The Engine and Vesting contracts have separate pause boundaries, but acquisition readiness also checks that the later RIPE settlement path is available:

* Pausing the Engine stops new acquisitions but does not stop claims.
* Pausing Vesting stops claims and also makes the Engine reject new acquisitions.
* RIPE token controls, the Engine's mint authorization, and the protocol minting circuit breaker can block claim settlement; the Engine also requires that mint path to be ready before accepting a new acquisition even though it does not mint RIPE at acquisition time.

Foxtrot is the intended named governance route. Its Engine-configuration and Vesting-allocation-budget changes are timelocked, while start, stop, payment-token changes, acquisition-availability changes, and rate-override installation or cancellation are immediate; changing the payment token still requires the Engine to be stopped. At the target contracts, these setters accept any registered Switchboard, so Foxtrot-only routing and its timelock are not independently enforced by the Engine or Vesting contract.

Replacing the Vesting contract while positions remain outstanding would require a separately implemented liability-preserving migration path; the current contracts do not provide one. Without such a path, governance must keep or restore the prior Vesting target until those positions can be claimed. A Vesting contract is retireable only when it is paused and no RIPE allocation remains outstanding.

## What to Check Before Acquiring

The preview is a quote, not a guarantee that a later transaction will succeed. Before execution, verify:

* The configured payment token and your allowance
* The quoted epoch, vesting duration, RIPE output, and deadline
* Remaining epoch capacity and Vesting allocation budget
* The claim-start and maturity schedule
* Whether you want direct RIPE delivery or an optional RipeGov deposit when claiming

The accepted payment becomes a protocol treasury asset immediately, while RIPE arrives only as the position vests and claims succeed. Treat the vesting schedule, token controls, mint authorization, and smart-contract risk as part of the acquisition.
