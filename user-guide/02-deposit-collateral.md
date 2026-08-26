---
description: Deposit Stock Tokens and other supported collateral to back one unified loan
---

# Deposit Stock Tokens and Other Collateral

A supported Robinhood Stock Token — called a **Stock Token** below — can join other eligible [collateral](../core-protocol/03-collateral-assets.md) in one GREEN loan. Each asset follows its configured terms; only deposits with nonzero borrowing terms add borrowing power.

**Step 1.** Go to [app.ripe.finance/robinhood](https://app.ripe.finance/robinhood) and press **Connect Wallet** (top right).

**Step 2.** Open the **Borrow** page from the left menu. It lists the collateral assets configured for that network and shows each asset's terms in the table.

![The Borrow page with the collateral table](../.gitbook/assets/user-guide-02-borrow-page.png)

**Step 3.** Find the asset you hold and press **+** on its row. Your wallet balance shows in the **Available in Wallet** column, so you can see what you have to work with.

![The deposit dialog, with your wallet balance shown](../.gitbook/assets/user-guide-05-deposit-modal.png)

**Step 4.** Enter the amount. The first time you deposit any asset, your wallet asks you to **Approve** it, a one-time permission. Confirm the approval, then confirm the deposit.

**Step 5.** Done. Your deposit shows in the **Your Deposits** column, and the panel at the top of the page updates with your totals.

One thing you'll notice: **Your Total Deposits** and **Your Total Collateral** can differ. A deposit contributes borrowing power only when its configured LTV is nonzero. A zero-LTV position can still have another protocol role—for example, earning rewards, participating in Stability settlement, or being eligible for a debt-reduction path. Treat the Borrow page and [RIPE Params](https://params.ripe.finance) as the live answer for supported assets and terms.

![Your position after depositing](../.gitbook/assets/user-guide-06-position-dashboard.png)

You can combine supported Stock Tokens with other supported assets in one portfolio. Deposits with nonzero borrowing terms contribute to the capacity of a [single loan](../core-protocol/02-borrowing.md).

Next: [Borrow GREEN](03-borrow-green.md).
