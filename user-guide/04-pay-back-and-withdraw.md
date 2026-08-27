---
description: Repay GREEN and take your collateral back out
---

# Pay Back and Withdraw

**Step 1.** Go to the **Borrow** page and press **Payback** (next to the Borrow button). It only appears once you have outstanding debt; with none, the panel simply reads No Debt. The Dashboard offers the same action under the name **Repay**. Same thing, either works.

![The payback dialog, showing Approve on first use](../.gitbook/assets/user-guide-10-payback-modal.png)

**Step 2.** Choose where the money comes from and how much. Two sources, and they work differently:

* **From your wallet:** GREEN or [sGREEN](../earning-and-rewards/01-sgreen.md). sGREEN is converted to GREEN and applied to your debt. The button reads **Approve** the first time — that's the token permission; approve it and the button becomes Payback. The MAX button pays everything you owe and refunds any excess (as sGREEN).
* **From your Stability Pool deposits:** flip the **Source** toggle to pay straight from sGREEN or LP tokens you've already deposited on Earn. This runs [deleverage](../core-protocol/05-deleverage.md) rather than a normal repayment: your pool position is spent to pay down the debt, no approval needed. It's the way to clear debt without touching your wallet — and it never touches your stock tokens.

Your debt ratio improves as soon as the payment lands.

**Step 3.** To take collateral out, use the **-** control on the asset's row in the collateral table (the **+** deposits, the **-** withdraws). You can withdraw anything not needed to cover your remaining debt, with a small buffer: Ripe keeps your debt just under your borrowing limit, so you can't withdraw right up to the edge. Repaid everything? Then you can withdraw everything. Withdrawals are blocked while a position is in liquidation — repay or add collateral to get out of it first.

**Step 4.** Confirm in your wallet, then check your wallet balance.

![The borrow page with no outstanding debt](../.gitbook/assets/user-guide-07-borrow-page-nodebt.png)

Next: [Get GREEN and provide liquidity](05-provide-liquidity.md).
