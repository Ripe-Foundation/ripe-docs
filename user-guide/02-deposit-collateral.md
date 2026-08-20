---
description: Deposit assets on the Borrow page to back one unified loan
---

# Deposit Collateral

Anything you deposit on the Borrow page becomes [collateral](../core-protocol/03-collateral-assets.md) for one loan.

**Step 1.** Go to app.ripe.finance/robinhood and press **Connect Wallet** (top right).

**Step 2.** Open the **Borrow** page from the left menu. It lists every asset you can deposit: the six stock tokens (SPCX, NVDA, TSLA, AAPL, GOOGL, GME) and WETH, each with its terms shown in the table.

![The Borrow page with the collateral table](../.gitbook/assets/user-guide-02-borrow-page.png)

**Step 3.** Find the asset you hold and press **+** on its row. Your wallet balance shows in the **Available in Wallet** column, so you can see what you have to work with.

![The deposit dialog, with your wallet balance shown](../.gitbook/assets/user-guide-05-deposit-modal.png)

**Step 4.** Enter the amount. The first time you deposit any asset, your wallet asks you to **Approve** it, a one-time permission. Confirm the approval, then confirm the deposit.

**Step 5.** Done. Your deposit shows in the **Your Deposits** column, and the panel at the top of the page updates with your totals.

One thing you'll notice: **Your Total Deposits** and **Your Total Collateral** can differ. Not everything you deposit adds borrowing power. On Robinhood Chain, the assets that do are currently the six stock tokens and WETH. Earn-side positions like RIPE, LP tokens, and [sGREEN](../earning-and-rewards/01-sgreen.md) carry no borrowing limit, so they don't raise what you can borrow. They aren't walled off from your loan either, though. If your position is ever [liquidated](../core-protocol/04-liquidations.md), those deposits can be drawn on too, the GREEN-side ones especially. More assets are expected to be added over time, so treat the Borrow page table as the live answer rather than this list.

![Your position after depositing](../.gitbook/assets/user-guide-06-position-dashboard.png)

You can deposit several assets, mixing and matching stocks and WETH however you like, and each new deposit increases what you can borrow. They all back a [single loan](../core-protocol/02-borrowing.md).

Next: [Borrow GREEN](03-borrow-green.md).
