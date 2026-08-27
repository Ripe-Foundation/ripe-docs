---
description: Borrow GREEN against your stock tokens, and keep the position healthy
---

# Borrow GREEN

[GREEN](../core-protocol/01-green-stablecoin.md) is the dollar stablecoin you borrow against your deposits. The app shows each asset's rate and borrowing limit in the table, and your own numbers before you confirm anything.

**Step 1.** With collateral deposited, stay on the **Borrow** page. The panel at the top shows your position: Your Total Deposits, Your Total Collateral, Outstanding Debt, Available to Borrow, and the Borrow Rate.

**Step 2.** Press **Borrow** (top right of that panel).

![The borrow dialog, showing the receive and destination choices](../.gitbook/assets/user-guide-09-borrow-modal.png)

**Step 3.** Enter the amount. Borrow comfortably less than the maximum. Prices move, and borrowing less keeps your position healthier when they do. The bar shows your debt against the Max Borrow and Liquidation markers as you type.

**Step 4.** Choose what you receive and where it goes. This is easy to miss and it matters:

* **Receive Token:** **GREEN** (the plain stablecoin) or **Savings GREEN** ([sGREEN](../earning-and-rewards/01-sgreen.md), whose GREEN value rises with protocol revenue).
* **Destination:** **Wallet**, or **Stability Pool** to put it straight to work [earning](../earning-and-rewards/02-stability-pools.md). The Stability Pool option only applies to Savings GREEN: the borrow wraps your GREEN into sGREEN and deposits it in one go. Plain GREEN always lands in your wallet.

If you're borrowing to spend, choose GREEN and Wallet. If you're borrowing to earn, Savings GREEN into the Stability Pool saves you a separate deposit.

**Step 5.** Press **Borrow** and confirm in your wallet.

![Your position with an active loan, shown as Healthy](../.gitbook/assets/user-guide-08-borrow-page-healthy.png)

**What to watch afterward:** your Debt Status on the Borrow page (and Debt Ratio on the Dashboard). It reads **No Debt** before you borrow and **Healthy** while you have comfortable room, with your debt shown against the Max Borrow and Liquidation markers. If your collateral loses enough value, your position can be [liquidated](../core-protocol/04-liquidations.md).

The Dashboard shows the same position as a **Debt Ratio** card, which spells out how the number is calculated and the ratio at which liquidation happens. Same information, two vocabularies.

![The Dashboard's view of an active loan, with the Debt Ratio explained](../.gitbook/assets/user-guide-16-dashboard-debt.png)

**Stock collateral behaves differently on weekends.** Stock price feeds follow market hours, so when the market closes your stock collateral holds its last price inside Ripe for as long as the feed's freshness window allows (if the window runs out first, the token has no price until reopening and your account waits, repay-only). Holding Friday's price does not make you safe during the pause. You can still be liquidated in that window in two ways: if your position was already at the liquidation point when the feed stopped, or if other collateral that keeps moving — WETH, for instance — falls far enough to push you there on its own.

And when the market reopens, the stock price updates in one step rather than drifting. If the stock fell while markets were closed, your position absorbs the whole move at once. Borrow with enough room that a weekend gap doesn't decide anything for you. [Stock Tokens on Ripe](../core-protocol/00-stock-tokens.md#market-hours-and-weekend-gaps) has the full picture, including what happens if a price goes missing entirely.

Next: [Pay back and withdraw](04-pay-back-and-withdraw.md).
