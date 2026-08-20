---
description: Wallet, gas and tokens — what to have ready before your first deposit
---

# Getting Set Up

Simple tutorials for using Ripe. The [protocol docs](../README.md) explain how Ripe works under the hood; these explain what to click. Each guide stands alone.

These walkthroughs follow the Ripe deployment on **Robinhood Chain**, so the setup below, the token names (USDG, the stock tokens) and the app URL are specific to that chain. The flows themselves — deposit, borrow, repay, earn — work the same way wherever Ripe runs.

You need three things: a wallet connected to Robinhood Chain, a little ETH for gas, and something to deposit.

**Step 1.** Add Robinhood Chain to your wallet using the official guide at docs.robinhood.com/chain. Never use a network config from a search result or a DM.

**Step 2.** Bridge ETH in from Ethereum or Base. Use the canonical bridge linked from docs.robinhood.com/chain/bridging. Your ETH arrives as ETH on Robinhood Chain, which is what pays for transactions.

**Step 3.** Keep a small amount of ETH for gas. Swap or wrap the rest into what you plan to use: WETH or [stock tokens](../core-protocol/03-collateral-assets.md) if you're depositing collateral, USDG if you're [providing liquidity](05-provide-liquidity.md). Swaps happen on the chain's DEXes, or from the Get buttons inside the Ripe app.

If you already hold stock tokens in your wallet on Robinhood Chain, your deposit is ready. You still need a little ETH for gas, though, and gas ETH arrives the same way as everything else: over the bridge.

**One safety rule for everything below:** only use token addresses shown inside the app or on ripe.finance. Lookalike tokens with the same names exist. If you found an address anywhere else, don't use it.

![The Ripe dashboard on Robinhood Chain](../.gitbook/assets/user-guide-01-dashboard.png)

Next: [Deposit collateral](02-deposit-collateral.md).
