---
description: Wallet, gas and verified Robinhood Stock Tokens — what to have ready before your first deposit
---

# Getting Set Up

Simple tutorials for using Ripe. The [protocol docs](../README.md) explain how Ripe works under the hood; these explain what to click. Each guide stands alone.

These walkthroughs follow Ripe on **Robinhood Chain** and begin with Robinhood Stock Tokens, called **Stock Tokens** below. The app URL and the assets and terms shown there are specific to that deployment. The flows themselves — deposit, borrow, repay, earn — work the same way wherever Ripe runs.

You need three things: a wallet connected to Robinhood Chain, a little native ETH to pay transaction gas, and a supported Stock Token or other collateral asset shown in the Ripe app.

**Step 1.** Add Robinhood Chain to your wallet using the [official network guide](https://docs.robinhood.com/chain/add-network-to-wallet/). Never use a network config from a search result or a DM.

**Step 2.** Fund the wallet with enough native ETH to pay gas using a route identified in the [official Robinhood Chain bridging documentation](https://docs.robinhood.com/chain/bridging/). Network and bridge entry points can change, so verify the route there before signing.

**Step 3.** Before acquiring or depositing a Stock Token, verify its contract address in the [official Robinhood Stock Token registry](https://docs.robinhood.com/chain/contracts/). Then confirm that the same asset appears as supported in the [Ripe app](https://app.ripe.finance/robinhood) and [RIPE Params](https://params.ripe.finance). The official registry is the live authority for token identity; the Ripe app and RIPE Params show current protocol support and terms. Do not infer any of those facts from an example or token name in these docs.

If you need to acquire a Stock Token, start with the current [official Robinhood Stock Token documentation](https://docs.robinhood.com/chain/stock-tokens/) for issuer information. Ripe is not the issuer. A Ripe interface or permitted protocol route may facilitate a transfer, but that does not establish issuance authority or investor eligibility. Before signing, verify the exact token identity, current Ripe support, the route provider, and your eligibility under RHJ's current [Base Prospectus, supplements, and applicable Final Terms](https://docs.robinhood.com/rhj).

Already holding a token with the right name is not enough. Confirm the exact contract and current Ripe support before trying to deposit, and keep enough ETH in the wallet to pay for each transaction.

**One safety rule for everything below:** reconcile the official Robinhood registry with the Ripe app and RIPE Params before using a token address. Lookalike tokens can share a name or symbol.

![The Ripe dashboard on Robinhood Chain](../.gitbook/assets/user-guide-01-dashboard.png)

Next: [Deposit collateral](02-deposit-collateral.md).
