---
description: Quick answers to what actually matters.
---

# Frequently Asked Questions

> **Live terms live onchain.** Which assets are supported, their LTVs, rates, fees, and which optional features are switched on vary by deployment and change over time. Numbers below are examples. [Params](https://params.ripe.finance) is the source of truth.

## Getting Started

### What is Ripe Protocol?

Borrow against your stocks without selling them. Deposit tokenized stocks — plus your ETH, stablecoins, and other supported assets — and borrow GREEN, a dollar stablecoin, against all of it in ONE loan.

### What makes Ripe different from other lending protocols?

**One loan, many assets.** Other protocols make you manage a separate position for every collateral type, and most won't take a tokenized stock at all. On Ripe, your stock tokens, ETH, stablecoins, and yield positions all back a single GREEN [borrowing position](../core-protocol/02-borrowing.md) at one blended rate, with one number to watch.

### What is GREEN?

[GREEN](../core-protocol/01-green-stablecoin.md) is Ripe's overcollateralized stablecoin, targeting $1. You mint it by borrowing against your deposits and burn it by repaying. There's no lender on the other side.

### Is my money safe?

Your collateral backs your loan and nobody else's. If someone else's position blows up, nothing happens to yours. Stability Pool deposits are different by design: that liquidity is what buys liquidated collateral, so it's spent on other people's liquidations in exchange for their collateral at a discount. See [Collateral Assets](../core-protocol/03-collateral-assets.md), [Stability Pools](../earning-and-rewards/02-stability-pools.md), and the risks list at the bottom of this page.

## Borrowing Basics

### Can I borrow against tokenized stocks without selling them?

Yes. That's the whole point. Deposit a supported stock token and borrow GREEN against it; the tokens sit in Ripe one-for-one and keep their full upside. Ripe isn't the issuer of any stock token, and issuers set their own rules on who can hold them — [Stock Tokens on Ripe](../core-protocol/00-stock-tokens.md) explains how they're held, priced, and where to read the issuer's terms.

### How much can I borrow?

Each asset has its own Loan-to-Value ratio; safer assets get higher LTVs. Your total [borrowing power](../core-protocol/02-borrowing.md) is the sum of each deposit's value × its LTV. The Borrow table in the app and [Params](https://params.ripe.finance) show the live numbers.

### How do interest rates work?

You pay a single weighted-average rate based on your [collateral mix](../core-protocol/03-collateral-assets.md). Mostly stablecoins with a little ETH? Your rate sits close to the stablecoin rate. Your rate is stored on your loan and refreshes when you borrow, repay, or your position is re-checked.

### What are dynamic rates?

A defense mechanism for GREEN's peg. If GREEN sits oversupplied in its reference pool for a sustained stretch, borrowing rates rise (with a hard cap) to encourage repayment. One bad reading doesn't trigger it, and rates fall back once the pool has been healthy for a while. See [Dynamic Interest Rates](../core-protocol/02-borrowing.md#dynamic-interest-rates).

### Can I repay anytime?

Yes. No term, no prepayment penalty, partial payments welcome. Pay with GREEN or sGREEN from your wallet, or use your Stability Pool deposits through [deleverage](../core-protocol/05-deleverage.md).

## Managing Risk

### What happens to my stock tokens when the market is closed?

Stock price feeds follow market hours, so over a weekend your stock collateral holds Friday's last price for as long as the feed's freshness window allows — and if the window is shorter than the closure, the token has no price until Monday and your account waits, repay-only. You can still be liquidated in that window if you were already past the threshold or your other collateral keeps falling. When the market reopens, the new price lands in one step. Borrow with room. Full explanation: [Stock Tokens on Ripe](../core-protocol/00-stock-tokens.md#market-hours-and-weekend-gaps).

### What happens if Ripe can't price one of my assets?

Ripe fails closed rather than guessing. While any of your borrowing collateral has no usable price you can't borrow, and every action that re-values your account — deposits, withdrawals, liquidations, deleverage — reverts until a price is back. Redemption is the exception: it doesn't revert, it just skips your account. Repaying still works, and everything resumes when a good price returns. See [When an Account Cannot Be Valued](../core-protocol/06-price-oracles.md#when-an-account-cannot-be-valued).

### Can I lose my stock tokens while borrowing?

Borrowing never sells them. In the ordinary course your tokens leave Ripe in exactly three ways: you withdraw them, a GREEN holder redeems against your position (only once you're in the redemption zone, at fair value), or you're liquidated (only past the liquidation threshold). Ordinary deleverage doesn't touch them. Governance also holds two emergency tools that can move any collateral at oracle value; nobody else can trigger those. Keep your position healthy and they stay yours. See [Which Events Can Move Your Tokens](../core-protocol/00-stock-tokens.md#which-events-can-move-your-tokens).

### When do I get liquidated?

When your collateral value falls to the liquidation threshold for your debt, anyone can trigger a [liquidation](../core-protocol/04-liquidations.md). Three thresholds to know, from safest to worst:

1. **Max LTV**: your borrowing limit — you can't borrow past it
2. **Redemption threshold**: GREEN holders can redeem against your position at fair value
3. **Liquidation threshold**: a liquidation can start

**Example:** $6,000 of debt against collateral with an 80% liquidation threshold needs more than $7,500 ($6,000 ÷ 0.80). At or below that, you're liquidatable.

Watch your position and add collateral or repay before you get there. [How Thresholds Work Together](../core-protocol/02-borrowing.md#how-thresholds-work-together-a-visual-guide) has the visual.

### What's the difference between redemption and liquidation?

**Redemption** comes first and costs you nothing extra: a GREEN holder pays down your debt at $1 per GREEN and takes collateral at oracle price. No discount, no fee — it deleverages you at fair value.

**Liquidation** is the last resort: liquidation and keeper fees apply, and your collateral is sold through [Stability Pools and Dutch auctions](../core-protocol/04-liquidations.md) at a discount.

### How does partial liquidation work?

Ripe only liquidates enough to make your position healthy again, not the whole thing. If you need a 20% debt reduction to be safe, that's what gets liquidated and you keep the rest. The one exception: if you're deeply underwater, the target becomes your full debt and all liquidation-eligible collateral can go and still leave debt.

## Earning with Ripe

### What is sGREEN?

[sGREEN](../earning-and-rewards/01-sgreen.md) is yield-bearing GREEN. Your sGREEN balance stays the same while its GREEN value rises from origination fees and borrower interest. No staking, no claiming — hold it and it grows.

### How do Stability Pools work?

[Stability Pools](../earning-and-rewards/02-stability-pools.md) hold sGREEN and GREEN LP tokens. When a position is liquidated, the pool buys the collateral below oracle price and you own your share of it. Depositors keep earning on the deposited asset, receive liquidated collateral at a spread, and earn RIPE rewards — like being a liquidator without running any bots.

### How do I earn RIPE rewards?

Borrow GREEN, lock RIPE, or deposit into a Stability Pool. [RIPE rewards](../earning-and-rewards/03-ripe-rewards.md) accrue every block, weighted by size × time, and mint when you claim. Part of every claim is auto-staked into the governance vault with a lock.

## GREEN Stability

### How does GREEN maintain its $1 peg?

Six mechanisms work together:

1. **Overcollateralization**: every GREEN is minted against more collateral than debt
2. **Dynamic rates**: borrowing gets pricier when GREEN is oversupplied
3. **Direct redemption**: GREEN redeems for $1 of collateral from positions in the redemption zone
4. **Stability Pool redemption**: GREEN redeems for $1 of liquidated collateral held by the pools
5. [**Endaoment**](../core-protocol/07-endaoment.md) **operations**: the treasury adds or removes GREEN liquidity to push the pool back toward balance
6. **Peg Stability Module (PSM)**: swap GREEN and a reserve stablecoin around $1, when enabled

### What happens if GREEN trades below $1?

Arbitrageurs buy cheap GREEN and redeem it — through the PSM, against positions in the redemption zone, or against Stability Pool collateral — for $1 of value. That buying pressure pushes GREEN back toward $1. Each path depends on there being something to redeem against, which is why there are several.

### Can GREEN lose its peg permanently?

No mechanism can guarantee a market price. What Ripe guarantees is the incentive: the further GREEN trades from $1, the bigger the arbitrage for whoever fixes it, and every GREEN loan starts overcollateralized.

### What is the PSM and how do I use it?

The [Peg Stability Module](../core-protocol/01-green-stablecoin.md#6-peg-stability-module-psm) swaps GREEN and its reserve stablecoin around $1 in either direction, with optional fees, per-interval limits, and — for redemptions — whatever reserves it holds. Each direction is switched on separately by governance; the app and [Params](https://params.ripe.finance) show whether it's live on your network.

## Technical Details

### Why did my transaction fail?

Most likely you did two things in one block. Withdrawals, borrows, Stability Pool claims, and releasing a governance lock early are limited to one per block per account — and any earlier action in that block, a deposit or a repayment included, uses up the slot. Wait a moment and retry. If it keeps failing, check whether one of your collateral assets has lost its price (see "What happens if Ripe can't price one of my assets?").

### What is account locking?

An emergency control that blocks deposits, withdrawals, borrows, and repayments for a specific wallet — used when an account looks compromised. It covers the main app actions, not every contract (the PSM and Reserve Engine have their own controls). Contact the team on Discord to resolve it.

### Can the protocol be paused?

Yes, component by component. Deposits, borrowing, liquidations, claims, the PSM, price sources, and minting each have their own switch, so an emergency pause on one doesn't necessarily stop the others. Pauses are for genuine emergencies.

## Advanced Features

### Can I use yield-bearing tokens as collateral?

Yes. Supported yield-bearing assets — liquid staking tokens, lending positions, vault shares — keep earning while they back your loan. Which ones are supported, and in which vault, is on the Borrow table and [Params](https://params.ripe.finance).

### What's the delegation system?

You can grant specific permissions to other addresses:

**Per-address delegations:**
* `canWithdraw`: withdraw collateral on your behalf (it always lands in your wallet)
* `canBorrow`: borrow and [deleverage](../core-protocol/05-deleverage.md) on your behalf
* `canClaimFromStabPool`: claim liquidated collateral from Stability Pools for you
* `canClaimLoot`: claim RIPE rewards for you

**Account-wide settings:**
* `canAnyoneDeposit`: let any address add collateral for you
* `canAnyoneRepayDebt`: let anyone repay your debt (handy for rescues)
* `canAnyoneBondForUser`: let others buy bonds for you

Each permission is independent and you can change them any time. Withdrawals always go to you — but a delegate with borrow or withdraw rights can still change your risk, so grant them carefully.

### Can I use my borrowed GREEN to earn yield?

Absolutely. When borrowing you can receive plain GREEN, convert it to sGREEN, or send sGREEN straight into a Stability Pool. If sGREEN's yield beats your borrow rate you're earning positive carry — but neither side of that spread is fixed.

## Governance & RIPE Token

### What is RIPE?

RIPE is the protocol's [governance](../governance-and-economics/02-governance.md) token. Lock it in the governance vault to build governance points (your future voting weight), earn from the staker rewards allocation, and boost your rate with longer locks.

### When does governance go live?

Governance points are accumulating now; onchain voting isn't live yet. When it ships, the points you've built become your voting weight. See [Governance](../governance-and-economics/02-governance.md).

### What's the RIPE token distribution?

1 billion RIPE, total, across every chain Ripe runs on — one cap, not one per network:

* **25%** Community incentives (the only allocation unlocking at TGE)
* **22.2%** Ripe Foundation treasury
* **20.6%** Core contributors (4-year linear vesting)
* **17.2%** Early backers ($550k seed at $0.02)
* **15%** Distribution partner (Hightop)

Investors and employees have agreed to an extra year of lock on top of their vesting. Details in [RIPE Tokenomics](../governance-and-economics/01-ripe-tokenomics.md).

### What are Ripe Bonds?

[Bonds](../governance-and-economics/03-bonds.md) let you swap a stablecoin for RIPE at the current epoch price. Lock the RIPE for a bonus; add a Bond Booster if you've earned one. Your payment goes to the [Endaoment](../core-protocol/07-endaoment.md) treasury.

### What is the RIPE Reserve Engine?

A second way to acquire RIPE: pay up front, choose a vesting length, and get a larger allocation for a longer wait. RIPE is minted as it vests and you claim it. See [RIPE Reserve Engine](../governance-and-economics/04-reserve-engine.md).

### What are Bond Boosters?

Bonus RIPE on bond purchases for ecosystem contributors (testnet participants, for example), up to a set number of units per person and for a limited time. See [Ripe Bonds](../governance-and-economics/03-bonds.md).

### What's the Endaoment?

The [Endaoment](../core-protocol/07-endaoment.md) is Ripe's treasury. Bond and Reserve Engine payments land there, and governance can deploy it for yield through [Underscore](https://underscore.finance/) integrations, provide GREEN liquidity, and run peg-defense operations.

## Safety & Security

### What are the main risks?

* **Smart contract risk**: bugs could affect funds
* **Liquidation risk**: collateral value dropping too fast — including a weekend gap in a stock price
* **Oracle risk**: a wrong or missing price; stock tokens usually have a single feed
* **Interest rate risk**: dynamic rates during market stress
* **Issuer risk**: a stock token is issued by a third party whose product terms, rights, and eligibility rules apply, and who can pause or freeze the token — see [Stock Tokens on Ripe](../core-protocol/00-stock-tokens.md)

### Is Ripe audited?

ChainSecurity reviewed an earlier Ripe Finance smart-contract architecture, and Anatomist reviewed a later Ripe Protocol revision. Each review covers only the specific code revisions and scope identified in its report; later changes and deployments may not be covered. See [Audits](audits.md) for the published reports and scope details.

### How does Ripe price assets accurately?

Ripe checks its [price sources](../core-protocol/06-price-oracles.md) in priority order and uses the first good answer. A failing source doesn't block the next one. If nothing can price an asset, Ripe stops rather than guesses.

### How does Ripe handle bad debt?

Layers: conservative LTVs, redemption and deleverage before liquidation, [liquidation](../core-protocol/04-liquidations.md) through funded [Stability Pools](../earning-and-rewards/02-stability-pools.md), then auctions. If bad debt still happens and governance records it, [bond](../governance-and-economics/03-bonds.md) purchases clear it — and that recovery RIPE is the one case where issuance can go beyond the 1 billion cap, socializing the shortfall across holders.

### What happens in a market crash?

* Redemptions and deleverage give positions two ways to shed debt before liquidation
* Stability Pools absorb liquidations as long as they're funded
* Liquidations take only what's needed — unless a position is deeply underwater
* Dynamic rates push borrowers to repay if GREEN weakens
* Prices come from the first working source; if none works, Ripe stops rather than guesses

## Getting Help

### Where can I learn more?

* [**Documentation**](../): Detailed guides in our docs
* [**Discord**](https://discord.gg/hightop): Active community and team support
* [**Twitter**](https://x.com/ripe_dao): Protocol updates and announcements
* [**GitHub**](https://github.com/Ripe-Foundation/ripe-protocol): Open source code and development
* [**Homepage**](https://www.ripe.finance/): Ripe Homepage

### How do I report bugs or issues?

* **Security issues**: report privately to ripefinance@proton.me
* **General bugs**: open an issue on GitHub or report in Discord

***

_This FAQ covers common questions. For detailed technical information, see our comprehensive documentation._
