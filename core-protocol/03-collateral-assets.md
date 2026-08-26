---
description: Stock tokens, blue-chip crypto, stablecoins, yield positions — all backing one loan.
---

# Collateral Assets

Start with your stock tokens. Add your WETH, your stablecoins, your staked ETH. Ripe treats the lot as one portfolio backing one GREEN loan, with each asset contributing its own terms. No vault per asset. No idle capital. Your collateral backs your loan and nobody else's.

> **Live terms live onchain.** Which assets are supported, their LTVs, deposit caps, and which optional features are switched on vary by deployment and change over time. Every number on this page is an example. [Params](https://params.ripe.finance) is the source of truth.

## One Portfolio, One Loan

Most DeFi lending forces a choice. Isolated markets give you one loan per asset — five assets, five positions, five liquidation risks. Pooled lending makes your deposits back everyone else's loans, so it only accepts a short list of "safe" tokens.

Ripe does neither:

```
Your portfolio                         Ripe
┌──────────────────────────┐    ┌──────────────────────────┐
│ Stock tokens    60% LTV  │    │ One GREEN loan           │
│ WETH            70% LTV  │ ─> │ Weighted rate & limits   │
│ Stablecoin      80% LTV  │    │ One health number        │
│ Staked ETH      70% LTV  │    │ Your risk, only yours    │
└──────────────────────────┘    └──────────────────────────┘
```

Each asset keeps its own LTV, rate, and thresholds; they blend into one position by borrowing power. See [Weighted Debt Terms](02-borrowing.md#weighted-debt-terms-explained).

## Stock Tokens as Collateral

Ripe holds stock tokens one-for-one: deposit 10 tokens and your position records 10 tokens. Borrowing GREEN against them never sells them, lends them out, or touches them — if the stock rises, your collateral rises with it. Each token is priced by its feed, corporate actions included; when the market is closed, the last price holds for as long as the feed's freshness window allows.

Everything stock-specific — custody, pricing, market hours, corporate actions, issuer terms, and exactly which events can move your tokens — is on [Stock Tokens on Ripe](08-stock-tokens.md).

## Asset Categories

Ripe can support any ERC-20 with a reliable price feed, once governance adds it. Terms are set per asset — a stablecoin gets a higher LTV than a volatile token — and every category below is a tier, not a list of what's live.

**1. Tokenized stocks and other real-world assets.** Stock tokens today; bonds, funds, and commodities as they come onchain. Some carry issuer-level transfer rules that Ripe respects.

**2. Stablecoins.** USDC and other dollar tokens, plus yield-bearing stables. The highest LTVs, because the price barely moves.

**3. Blue-chip crypto.** WETH, WBTC, and other established tokens with deep liquidity and reliable price feeds.

**4. Yield-bearing assets.** Staked ETH (stETH-type tokens), LP tokens, and vault shares. They keep earning while they back your loan — Ripe's share accounting credits the yield to you.

Supported [Underscore](https://underscore.finance/) vault shares are one example: they keep earning inside the vault while deposited on Ripe. The [Earn-vault rate discount](02-borrowing.md#underscore-earn-vault-integration) applies to the vault when it borrows, not to you for depositing its shares.

**5. Emerging assets.** Newer tokens with less history get conservative terms — a low LTV, or none at all.

## How Deposits Work

### Vault Types

Each asset is assigned to a vault whose accounting fits how the token behaves. You just deposit; Ripe picks the vault.

**Simple ERC-20 vaults** — plain tokens, including stock tokens. Your balance is the exact token count you deposited. These vaults are strict: a deposit only counts once the exact amount has arrived, and a withdrawal only completes if the exact amount can leave. If the token refuses a transfer, the transaction fails rather than half-completing.

**Rebase and share vaults** — yield-bearing tokens whose balances grow. You hold shares of the vault's balance, so yield accrues to you. Withdrawals from these vaults tolerate rounding dust and credit what actually leaves.

**Governance vault** — lock RIPE for [governance](../governance-and-economics/02-governance.md) power. Not collateral.

**Stability Pools** — deposit sGREEN or an LP token to [buy liquidated collateral](../earning-and-rewards/02-stability-pools.md) below market. Never collateral.

**Future vault types** — NFTs and other assets that need their own custody or accounting. No NFT vault exists yet; new vault types plug in without touching existing ones.

### Deposit Limits

Deposit limits are set per asset, in token units:

* **Per-user cap** — the most one account can hold of an asset in its vault. You can deposit up to the cap minus your current balance.
* **Global cap** — the most the vault can hold of that asset in total. Caps let an asset earn trust before it becomes a big share of GREEN's backing.
* **Minimum balance** — a floor so dust positions don't exist. A deposit or withdrawal has to leave you at zero or at least the minimum.
* **Max vaults and assets** — a ceiling on how many vaults one account can use and how many assets per vault.

Governance raises or lowers these over time.

## Making Withdrawals

Withdraw any time your position stays healthy. No queue, no waiting period, partial or full.

How much you can take out depends on the asset's LTV, your current debt with interest, and what your other collateral covers. Ripe keeps a 1% buffer: after the withdrawal, your debt has to sit at or under about 99% of your max borrow capacity.

**Example:**

```
Deposited:    $10,000 ETH at 70% LTV
Borrowed:     $5,000 GREEN

Required collateral = $5,000 × 1.01 ÷ 0.70 = $7,214.29  (1% buffer)
Available to withdraw ≈ $2,785.71 of ETH
```

Once you're over max LTV, no withdrawals go through — not even assets with a 0% LTV — until you repay or add collateral. Your tokens can also leave through redemption (in the redemption zone) or liquidation (past the liquidation threshold). See [Key Safety Thresholds](02-borrowing.md#how-thresholds-work-together-a-visual-guide).

## Earning While Deposited

Deposits can earn RIPE through the points system while rewards are switched on:

```
Points  = Deposit value × Blocks held
Share   = Your points / Total points
Rewards = Your share × Emissions
```

Time counts as much as size. Deposit categories, vote-selected bonuses, and Stability Pool and governance-vault rewards are all covered on [RIPE Rewards](../earning-and-rewards/03-ripe-rewards.md).

## Delegation

You can let another address act on your position. Two kinds of permission:

* **Account-wide, off by default:** `canAnyoneDeposit`, `canAnyoneRepayDebt`, `canAnyoneBondForUser`. Anyone can add collateral, repay, or bond for you — none of these can move value out.
* **Per delegate:** `canWithdraw`, `canBorrow`, `canClaimFromStabPool`, `canClaimLoot`. Grant them to a specific address; revoke any time.

Withdrawals always land in your wallet, even when a delegate triggers them. But a delegate with borrow or withdraw rights can still hurt your position — borrowing to your limit, or withdrawing the collateral that keeps you healthy. Delegate to addresses you control or trust.

Useful for team treasuries, automated strategies, and smart-wallet setups.

## Asset-Specific Permissions

* Some tokens carry issuer-level rules — allowlists, blocklists, pauses — that Ripe can't override.
* Governance can put an asset behind a whitelist; the app tells you if you need approval.
* Which exit paths an asset uses — redemption, Stability Pool, auction — is set per asset.

## Why Deposit in Ripe?

* **Every asset works.** Stock tokens, crypto, stablecoins, yield positions — one loan.
* **Collateral that keeps earning.** Staked and vault positions keep compounding while they back your debt.
* **Your risk is only yours.** Your collateral backs your loan, not a shared pool.
* **RIPE rewards** on deposits, no lock-up on ordinary collateral.

Don't sell your stocks. Put them to work.

***

_For technical implementation details, see the_ [_Teller Technical Documentation_](https://ripe-finance.gitbook.io/ripe-developers/core/teller)_._
