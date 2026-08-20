---
description: Borrow GREEN against your deposits, and keep the position healthy
---

# Borrow GREEN

[GREEN](../core-protocol/01-green-stablecoin.md) is the dollar stablecoin you borrow against your deposits. The app shows each asset's rate and borrowing limit in the table, and your own numbers before you confirm anything.

**Step 1.** With collateral deposited, stay on the **Borrow** page. The panel at the top shows your position: Your Total Deposits, Your Total Collateral, Outstanding Debt, Available to Borrow, and the Borrow Rate.

**Step 2.** Press **Borrow** (top right of that panel).

![The borrow dialog, showing the receive and destination choices](../.gitbook/assets/user-guide-09-borrow-modal.png)

**Step 3.** Enter the amount. Borrow comfortably less than the maximum. Prices move, and borrowing less keeps your position healthier when they do. The bar shows your debt against the Max Borrow and Liquidation markers as you type.

**Step 4.** Choose what you receive and where it goes. This is easy to miss and it matters:

* **Receive Token:** **GREEN** (the plain stablecoin) or **Savings GREEN** ([sGREEN](../earning-and-rewards/01-sgreen.md), which earns yield automatically).
* **Destination:** **Wallet** (it lands in your wallet, ready to spend or move) or **Stability Pool** (it goes straight to work [earning](../earning-and-rewards/02-stability-pools.md), with the APY shown right there).

If you're borrowing to spend the money, choose GREEN and Wallet. If you're borrowing to put the money to work, sending it straight to the Stability Pool saves you a separate deposit.

**Step 5.** Press **Borrow** and confirm in your wallet.

![Your position with an active loan, shown as Healthy](../.gitbook/assets/user-guide-08-borrow-page-healthy.png)

**What to watch afterward:** your Debt Status on the Borrow page (and Debt Ratio on the Dashboard). It reads **No Debt** before you borrow and **Healthy** while you have comfortable room, with your debt shown against the Max Borrow and Liquidation markers. If your collateral loses enough value, your position can be [liquidated](../core-protocol/04-liquidations.md).

The Dashboard shows the same position as a **Debt Ratio** card, which spells out how the number is calculated and the ratio at which liquidation happens. Same information, two vocabularies: the Borrow page gives you a word, the Dashboard gives you the percentage behind it.

![The Dashboard's view of an active loan, with the Debt Ratio explained](../.gitbook/assets/user-guide-16-dashboard-debt.png)

**Stock collateral behaves differently on weekends.** The [price feeds](../core-protocol/06-price-oracles.md) for stocks pause when markets close, so inside the protocol your stock collateral holds its last price for roughly two days. That does not make you safe during the pause. You can still be liquidated in that window in two ways: if your position was already at the liquidation point when the feed froze, or if other collateral that keeps moving, WETH for instance, falls far enough to push you there on its own.

And when feeds resume, the stock price updates in one step rather than drifting. If the stock fell while markets were closed, your position absorbs the whole move at once instead of giving you time to react. Borrow with enough room that a weekend gap doesn't decide anything for you.

Next: [Pay back and withdraw](04-pay-back-and-withdraw.md).
