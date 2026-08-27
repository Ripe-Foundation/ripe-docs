---
description: Wallet, gas, and stock tokens — what to have ready before your first deposit
---

# Getting Set Up

Simple tutorials for using Ripe. The [protocol docs](../README.md) explain how Ripe works under the hood; these explain what to click. Each guide stands alone.

The flows — deposit, borrow, repay, earn — are the same on every network Ripe runs on. The screenshots come from one deployment, so the exact token names, pairs, and terms you see will depend on the network you pick in the app. The app's tables and [Params](https://params.ripe.finance) are always the live answer.

You need three things: a wallet connected to a network Ripe is deployed on, a little of the network's gas token (ETH on current deployments), and something to deposit — ideally a stock token.

**Step 1.** Add the network to your wallet using that network's official documentation. Never use a network config from a search result or a DM.

**Step 2.** Bridge in a little gas using the network's canonical bridge. Keep some in reserve — every deposit, borrow, and claim is a transaction.

**Step 3.** Get the stock tokens you want to deposit. Two rules:

* **Verify the contract address** against the issuer's official token registry (Robinhood, for example, publishes its Stock Token registry at [docs.robinhood.com/chain/contracts](https://docs.robinhood.com/chain/contracts/)) and confirm the same token appears in the Ripe app's Borrow table. Lookalike tokens with the same name and symbol exist. If you found an address anywhere else, don't use it.
* **Check the issuer's terms.** Ripe isn't the issuer, and issuers set their own rules on who may hold their tokens. [Stock Tokens on Ripe](../core-protocol/00-stock-tokens.md) explains what a stock token is inside Ripe and where to read the issuer's product terms.

If you already hold stock tokens in your wallet, you're ready. You still need gas.

![The Ripe dashboard](../.gitbook/assets/user-guide-01-dashboard.png)

Next: [Deposit stock tokens](02-deposit-collateral.md).
