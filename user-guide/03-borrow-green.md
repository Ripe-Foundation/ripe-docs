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

* **Receive Token:** **GREEN** (the plain stablecoin) or **Savings GREEN** ([sGREEN](../earning-and-rewards/01-sgreen.md), a non-rebasing share whose GREEN backing can increase when configured revenue reaches the savings vault).
* **Destination:** **Wallet** (it lands in your wallet, ready to spend or move) or, when receiving **Savings GREEN** and the interface offers a compatible route, **Stability Pool** (the sGREEN is deposited into the configured Stability vault, with the estimate shown in the preview).

If you're borrowing to spend the money, choose GREEN and Wallet. If an eligible Stability destination is offered, choosing Savings GREEN and Stability Pool converts the borrowed GREEN to sGREEN and saves a separate deposit.

**Step 5.** Press **Borrow** and confirm in your wallet.

![Your position with an active loan, shown as Healthy](../.gitbook/assets/user-guide-08-borrow-page-healthy.png)

**What to watch afterward:** your Debt Status on the Borrow page (and Debt Ratio on the Dashboard). It reads **No Debt** before you borrow and **Healthy** while you have comfortable room, with your debt shown against the Max Borrow and Liquidation markers. If your collateral loses enough value, your position can be [liquidated](../core-protocol/04-liquidations.md).

The Dashboard shows the same position as a **Debt Ratio** card, which spells out how the number is calculated and the ratio at which the position becomes eligible for liquidation. Same information, two vocabularies: the Borrow page gives you a word, the Dashboard gives you the percentage behind it.

![The Dashboard's view of an active loan, with the Debt Ratio explained](../.gitbook/assets/user-guide-16-dashboard-debt.png)

**Stock collateral behaves differently when its market is closed.** A published stock price remains usable only while it satisfies the configured freshness rules. Ripe does not keep using a last-known cached price after every configured source becomes unusable.

If debt-bearing collateral cannot be priced, the account enters a valuation quarantine. You cannot increase risk by borrowing more or withdrawing collateral that supports the debt, and the protocol withholds liquidation, redemption, and deleveraging until it can value the account safely. You can still repay debt or add collateral, subject to normal protocol controls. Quarantine itself is not a liquidation or a declaration that the account is insolvent.

When a usable feed resumes, the account is valued from the fresh price and normal health checks resume. If the stock moved while its market was closed, the update may arrive in one step. Borrow with enough room that a market-open gap doesn't decide anything for you.

Next: [Pay back and withdraw](04-pay-back-and-withdraw.md).
