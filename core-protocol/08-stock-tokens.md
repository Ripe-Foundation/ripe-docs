---
description: Keep the tokens. Borrow the dollars.
---

# Stock Tokens on Ripe

A stock token is an ERC-20 that tracks the price of a public stock. Deposit it into Ripe and you can borrow GREEN against it — in the same loan as your ETH, your stablecoins, and everything else you hold — without selling a single token.

This page is the one place that explains how a stock token behaves inside Ripe: how it's held, how it's priced when the market is closed, what a dividend or split does, and exactly which events can move your tokens.

> **Live terms live onchain.** Which stock tokens are supported, their LTVs, rates, and price sources vary by deployment and change over time. Every number on this page is an example. [Params](https://params.ripe.finance) is the source of truth.

## What Happens When You Deposit

Ripe holds stock tokens one-for-one. Deposit 10 tokens and your position records 10 tokens — not a share of a pool, not a receipt. Borrowing GREEN doesn't sell them, lend them out, or touch them. If the stock goes up, your collateral goes up.

The vault is strict about it: a deposit only counts once the exact amount has arrived, and a withdrawal only completes if the exact amount can leave. If the token itself refuses a transfer (an issuer pause or blocklist, say), the transaction fails rather than half-completing.

Borrowing against them works like any other collateral: the token's LTV sets how much GREEN it adds to your borrowing power, and its rate and thresholds blend into your position's [weighted terms](02-borrowing.md#weighted-debt-terms-explained).

## How Stock Tokens Are Priced

Ripe values a stock token through its price sources in priority order and uses the first good answer, like any other asset. In practice a stock token usually has a single source: a Chainlink feed for that token. There is no second feed to fall back to, so if that feed goes stale the token has no price (see below).

**The feed prices the token, not the share.** Stock tokens carry an onchain multiplier that changes when a corporate action happens — a 2-for-1 split turns one token into two shares' worth, a cash dividend nudges the multiplier up. The feed already bakes that in: it reports the dollar value of one token. Ripe reads that number once and uses it. Your token count never changes; the price of each token does.

Ripe doesn't read the issuer's "oracle paused" flag or a market calendar. It just asks the feed for the latest price, checks that the round is valid and fresh enough, and uses it. That has a concrete consequence:

### Market Hours and Weekend Gaps

Stock feeds follow market hours. When the exchange closes for the weekend or a holiday, the feed stops updating and Ripe keeps using the last published price for as long as it's inside the feed's freshness window. Governance sets that window per feed — check it on [Params](https://params.ripe.finance); stock feeds are currently set long enough to span a normal weekend, and that setting can change.

So over a weekend:

* Your stock collateral holds Friday's close.
* You can still be liquidated in that window — if you were already past the threshold when the feed stopped, or if your other collateral (WETH, say) keeps falling and drags you there on its own.
* When the market reopens, the new price lands in one step. If the stock fell 8% while markets were closed, your position absorbs the whole 8% at once.

Borrow with enough room that a weekend gap doesn't decide anything for you.

### When the Price Goes Missing

If the feed's freshness window runs out — a long holiday, a corporate action that pauses the feed longer than expected, an outage — the token has no usable price. Ripe fails closed rather than guessing. While the feed is down:

* You can't borrow.
* Every action that re-values your account waits — deposits, withdrawals, liquidations, redemptions, deleverage — whether or not you have debt.
* You can still repay GREEN.

Everything resumes the moment a good price is back — and the first check after it returns uses the new price, gap and all.

## Which Events Can Move Your Tokens

Your stock tokens leave Ripe in three ordinary ways:

1. **You withdraw them.** Any time, as long as your position stays healthy after the withdrawal. (A delegate you've given `canWithdraw` can trigger this, but the tokens always go to you.)
2. **A redemption.** If your position slips into the [redemption zone](04-liquidations.md#the-redemption-buffer) and governance has enabled redemption for that token, GREEN holders can pay down your debt at $1 per GREEN and take tokens at oracle price. No discount, no penalty — it's a forced deleverage at fair value.
3. **A liquidation.** Past the liquidation threshold, anyone can trigger one. Your stock tokens go to a [Stability Pool](../earning-and-rewards/02-stability-pools.md) at the liquidation spread if that route is on for the token, and to a [Dutch auction](04-liquidations.md#phase-2-dutch-auctions) for whatever's left.

[Deleverage](05-deleverage.md) is the fourth way debt comes down, but it only spends your sGREEN and stablecoin positions — so deleveraging is how you *protect* your stock position, not how you lose it. Governance does hold two emergency tools that can move any collateral at oracle value (a volatile-asset deleverage and a collateral swap); they're governance actions, not something a keeper or another user can trigger.

## Issuer Freezes and Burns

A stock token is issued by a regulated entity that keeps control over the token contract. Two things can happen at the token level that Ripe can't prevent:

* **A transfer freeze** (pause, blocklist). Your tokens stay recorded in Ripe and can't be delivered out until the issuer lifts it, so withdrawals fail closed. Ripe's own bookkeeping keeps working in the meantime: a redemption or liquidation can still reassign the tokens to someone else inside the vault, and they're stuck there until the freeze ends.
* **A forced burn or transfer** out of the vault. If the vault holds fewer tokens than it has credited, Ripe treats the whole asset as unbacked: it contributes zero collateral value, and Ripe quarantines every account that holds it: no borrowing, no withdrawals while you have debt, no liquidation or redemption. You can still repay and add collateral, and if you have no debt you can still pull your other assets. The unbacked token itself can't leave until the backing is restored.

These are issuer risks, not Ripe risks, but they land on your position. Read the issuer's terms before you deposit.

## Issuers and Eligibility

Ripe is not the issuer of any stock token. Issuers structure their products differently and set their own rules on who may hold them: Robinhood Stock Tokens, for example, are debt securities issued by Robinhood Assets (Jersey) Limited that track a stock's price but carry no rights in the underlying shares, and they're not available to everyone. Ripe supporting a token doesn't change any of that. Check the issuer's own documentation for product terms, rights, and eligibility before you acquire or deposit a stock token — for Robinhood Stock Tokens, start at [docs.robinhood.com/chain/stock-tokens](https://docs.robinhood.com/chain/stock-tokens/).

## The Short Version

* Deposit stock tokens; borrow GREEN; keep every token.
* Priced by feed, per token, corporate actions included.
* Weekend prices hold; the reopen gap lands at once — leave room.
* No price means no borrowing, no withdrawing, no liquidating. Repay still works.
* Your tokens only leave when you withdraw them, when you're redeemed, or when you're liquidated.

***

_For technical implementation details, see the_ [_Price Desk_](https://ripe-finance.gitbook.io/ripe-developers/pricing/pricedesk) _and_ [_Chainlink Prices_](https://ripe-finance.gitbook.io/ripe-developers/pricing/chainlinkprices) _documentation._
