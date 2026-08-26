---
description: Repay GREEN and take your collateral back out
---

# Pay Back and Withdraw

**Step 1.** Go to the **Borrow** page and press **Payback** (next to the Borrow button). It only appears once you have outstanding debt; with none, the panel simply reads No Debt. The Dashboard offers the same action under the name **Repay**. Same thing, either works.

![The payback dialog, showing Approve on first use](../.gitbook/assets/user-guide-10-payback-modal.png)

**Step 2.** Choose the source and amount. The two source types use different protocol paths:

* **Wallet repayment:** Standard repayment accepts wallet GREEN or wallet [sGREEN](../earning-and-rewards/01-sgreen.md). Wallet sGREEN is redeemed to GREEN before the GREEN is applied to debt.
* **Deposited Stability position:** Selecting an eligible position already deposited in a [Stability vault](../earning-and-rewards/02-stability-pools.md) invokes [deleverage](../core-protocol/05-deleverage.md), not standard repayment. It can consume deposited sGREEN or another configured settlement asset under that route's permissions, ordering, valuation, and full-payoff rules.

For a wallet token, review and approve the requested spender and amount if prompted, then submit Payback. A deposited position generally does not use that wallet-token approval flow. In either case, debt changes only after the applicable transaction succeeds.

**Step 3.** To take collateral out, use the **-** control on the asset's row in the collateral table (the **+** deposits, the **-** withdraws). Ordinary withdrawal is unavailable while the account is in liquidation. Valuation quarantine blocks debt-supporting withdrawal capacity; a zero-LTV asset is not automatically blocked by the quarantine flag alone, but strict whole-account repricing and the normal vault, token, pause, and account controls can still make its withdrawal revert. Repay far enough below the current borrowing limit to satisfy the withdrawal buffer; a full repayment removes the debt-health constraint.

**Step 4.** Confirm in your wallet, then check your wallet balance.

![The borrow page with no outstanding debt](../.gitbook/assets/user-guide-07-borrow-page-nodebt.png)

Next: [Get GREEN and provide liquidity](05-provide-liquidity.md).
