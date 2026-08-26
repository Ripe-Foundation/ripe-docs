---
description: Borrow GREEN against Robinhood Stock Tokens and other supported collateral
---

# Borrow GREEN Against Stock Tokens and Your Portfolio

A supported Robinhood Stock Token — called a **Stock Token** below — can back the [GREEN](../core-protocol/01-green-stablecoin.md) you borrow alongside other eligible collateral. The app shows each asset's current terms and your position's numbers before you confirm anything.

**Step 1.** With collateral deposited, stay on the **Borrow** page. The panel at the top shows your position: Your Total Deposits, Your Total Collateral, Outstanding Debt, Available to Borrow, and the Borrow Rate.

**Step 2.** Press **Borrow** (top right of that panel).

![The borrow dialog, showing the receive and destination choices](../.gitbook/assets/user-guide-09-borrow-modal.png)

**Step 3.** Enter the amount. Borrow comfortably less than the maximum. Prices move, and borrowing less keeps your position healthier when they do. The bar shows your debt against the Max Borrow and Liquidation markers as you type.

**A closed stock market does not automatically quarantine Stock Token collateral.** The last published price may remain usable while it satisfies the configured freshness rules. If every configured source becomes unusable, an indebted account enters valuation quarantine only when the affected Stock Token has nonzero LTV and either a positive balance cannot be priced or a remaining nominal balance lacks usable backing. In that case, risk-increasing actions and new liquidation, redemption, and deleveraging processing wait until the account can be valued safely, while ordinary repayment remains available subject to its usual controls. A fresh reopening price can gap from the prior close, so borrow comfortably below the maximum. See [Stock-Market Hours and Price Gaps](../core-protocol/06-price-oracles.md#stock-market-hours-and-price-gaps) for the canonical explanation.

**Step 4.** Choose what you receive and where it goes. This is easy to miss and it matters:

* **Receive Token:** **GREEN** (the plain stablecoin) or **Savings GREEN** ([sGREEN](../earning-and-rewards/01-sgreen.md), whose backing per share can increase through configured protocol revenue).
* **Destination:** **Wallet** (it lands in your wallet, ready to spend or move) or **Stability Pool** (it enters a configured [liquidation-settlement pool](../earning-and-rewards/02-stability-pools.md), with current estimates and terms shown in the app).

If you're borrowing to spend the money, choose GREEN and Wallet. If you're borrowing to put the money to work, sending it straight to the Stability Pool saves you a separate deposit.

**Step 5.** Press **Borrow** and confirm in your wallet.

![Your position with an active loan, shown as Healthy](../.gitbook/assets/user-guide-08-borrow-page-healthy.png)

**What to watch afterward:** your Debt Status on the Borrow page (and Debt Ratio on the Dashboard). It reads **No Debt** before you borrow and **Healthy** while you have comfortable room, with your debt shown against the Max Borrow and Liquidation markers. If your collateral loses enough value, your position can be [liquidated](../core-protocol/04-liquidations.md).

The Dashboard shows the same position as a **Debt Ratio** card, which spells out how the number is calculated and the ratio at which liquidation happens. Same information, two vocabularies: the Borrow page gives you a word, the Dashboard gives you the percentage behind it.

![The Dashboard's view of an active loan, with the Debt Ratio explained](../.gitbook/assets/user-guide-16-dashboard-debt.png)

Next: [Pay back and withdraw](04-pay-back-and-withdraw.md).
