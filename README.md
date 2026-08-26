---
description: Don't sell your stocks. Borrow against them.
---

# Ripe Protocol: Don't Sell Your Stocks. Borrow Against Them.

Deposit tokenized stocks — and the rest of your portfolio — and borrow GREEN, a dollar stablecoin, against all of it in one loan. No bank. No credit check. No selling. Your shares stay yours, and so does the upside.

Wealthy families have borrowed against their portfolios for a century instead of selling. Ripe puts the same move onchain, for anyone holding a stock token.

> **Live terms live onchain.** Which assets are supported, their LTVs, rates, fees, and which optional features are switched on vary by deployment and change over time. Every number in these docs is an example. [Params](https://params.ripe.finance) is the source of truth.

***

## The Problem: DeFi Thinks Your Assets Live in Silos

**Isolated Vaults** (MakerDAO, Liquity) One asset, one vault. Five assets? Five loans. Five liquidation risks. We're still pretending portfolios don't exist.

**Shared Risk Pools** (Aave, Compound)\
Your collateral backs someone else's degen trade. Their bad debt? Your problem. And forget about using stock tokens at all.

**"Innovation" That Isn't** (Morpho, Euler) Slightly better rates. Same old isolation. Each market fragments liquidity further.

## The Ripe Solution: One Loan, Every Asset

```
Your Portfolio:             Traditional:           Ripe:
┌──────────────┐            ┌──────────────┐      ┌─────────────┐
│ Stock tokens │ ─────────> │ Stock loan   │      │             │
│ ETH          │ ─────────> │ ETH loan     │      │  ONE LOAN   │
│ Stablecoins  │ ─────────> │ Stable loan  │ ───> │  GREEN      │
│ Yield tokens │ ─────────> │ Yield loan   │      │  Everything │
│ Other assets │ ─────────> │ Other loan   │      │  working    │
└──────────────┘            └──────────────┘      └─────────────┘
                            Five positions         One position,
                            to babysit             one rate, one
                                                   number to watch
```

***

## Core Protocol

#### 📈 [Stock Tokens on Ripe](core-protocol/08-stock-tokens.md)

The whole point. What a stock token is inside Ripe, how it's priced when markets are closed, what corporate actions do, and exactly which events can move your tokens.

* **Keep the upside** — Borrowing never sells your tokens
* **Market hours, explained** — The last price holds through the weekend; the reopen gap lands in one step
* **Your tokens leave three ordinary ways** — You withdraw, a redemption, or a liquidation

#### 🟢 [GREEN: The Stablecoin That Actually Does Something](core-protocol/01-green-stablecoin.md)

USDC sits there. DAI does nothing. GREEN? It's working.

* **Minted, not lent** — Borrowing creates GREEN against your collateral; there's no lender on the other side
* **Targets $1** — Six mechanisms pull it back toward the peg, from arbitrage to treasury operations
* **Burns on repayment** — No infinite supply games

#### 💰 [Borrowing: One Loan, Your Whole Portfolio](core-protocol/02-borrowing.md)

Stop juggling vaults. Deposit, borrow, done.

* **One position** — Every asset with borrowing power backs the same loan, at one blended rate
* **Smart rates** — Your rate only rises when GREEN needs defending, and it's capped
* **Repay on your schedule** — No term, no prepayment penalty

#### 🏦 [Collateral: Everything Has a Job](core-protocol/03-collateral-assets.md)

Stock tokens, blue-chip crypto, stablecoins, yield-bearing positions. Each contributes its own terms to one loan.

* **Weighted terms** — Safer assets pull your rate and thresholds toward their numbers
* **Collateral that keeps earning** — Staked ETH keeps staking while it backs your loan
* **Your risk is only yours** — Your collateral backs your loan and nobody else's

#### 🛡️ [Liquidations: A Slice, Not the Plate](core-protocol/04-liquidations.md)

If a position goes underwater, Ripe takes what it needs to make it healthy again. You keep the rest.

* **Two warning zones first** — Redemption and deleverage give you room before liquidation
* **Stability Pools, then auctions** — Orderly settlement, no market dumps
* **Honest limit** — A severe shortfall can take all liquidation-eligible collateral and still leave debt

## Earning & Rewards

#### 💎 [sGREEN: Set It and Forget It](earning-and-rewards/01-sgreen.md)

Hold sGREEN and your balance's GREEN value rises as borrowers pay fees and interest.

* **Protocol revenue** — Origination fees and borrower interest flow to sGREEN
* **No lockup** — Redeem for GREEN whenever you like
* **Stackable** — Deposit sGREEN into a Stability Pool for liquidation proceeds on top

#### 🎯 [Stability Pools: Buy Liquidated Stock Tokens Below Market](earning-and-rewards/02-stability-pools.md)

Deposit once. When a position is liquidated, the pool buys the collateral at the liquidation spread and you own your share of it.

* **Liquidation spread** on every asset routed through the pool
* **Three income sources** — sGREEN yield, liquidation proceeds, RIPE rewards
* **No bots, no gas wars** — Deposit, wait, claim

#### 🪙 [RIPE Rewards: Get Paid to Use the Protocol](earning-and-rewards/03-ripe-rewards.md)

Borrow, stake, or deposit and you accrue RIPE every block.

* **Time-weighted** — Size × time, so a smaller position held longer can out-earn a whale's quick visit
* **Four categories** — Stakers, borrowers, depositors, voters
* **Auto-staking** — Part of every claim goes straight into governance

## Governance & Economics

#### 📊 [RIPE Tokenomics: Aligned Incentives](governance-and-economics/01-ripe-tokenomics.md)

* **1 billion RIPE, total, across every chain** — Bad-debt recovery counts against it, not on top of it
* **Community first** — 25% for users; the only bucket unlocking from day one
* **Team and investors locked longer** — An extra year on top of vesting
* **Bootstrapped** — $1.87M put to work, only $550K of it outside capital

#### 🏛️ [Governance: Power Through Time](governance-and-economics/02-governance.md)

Lock RIPE, stack governance points every block, and earn from the staker allocation while you wait.

* **Time multipliers** — Longer locks earn points faster
* **Staker rewards** — Locked RIPE earns from protocol emissions
* **Future control** — Points become voting weight when onchain voting ships

#### 🎟️ [Bonds: Fund the Future](governance-and-economics/03-bonds.md)

Trade stablecoins for RIPE at the epoch price. Lock it for a bonus. Your payment becomes treasury capital.

#### 🏦 [RIPE Reserve Engine: RIPE That Vests](governance-and-economics/04-reserve-engine.md)

A second way to acquire RIPE: pay up front, choose a vesting length, get a bigger allocation for a longer wait.

#### 🏰 [The Endaoment: A Treasury That Works](core-protocol/07-endaoment.md)

Bond and Reserve Engine payments become protocol-owned capital that governance can deploy for yield, liquidity, and peg defense.

## Resources

#### 🔮 [Price Oracles: The Truth About Your Money](core-protocol/06-price-oracles.md)

Ripe checks its price sources in priority order and uses the first good answer. No good answer? It fails closed rather than guessing.

#### 🛡️ [Security Audits: Published Assessments](resources/audits.md)

Independent security reviews of Ripe Protocol by ChainSecurity and Anatomist.

* **Independent reviews** - ChainSecurity and Anatomist
* **Full reports** - Both reports published in full
* **Dates and scope** - Reviewed code revisions and scope limits

#### 📚 [Protocol Archives: The Journey to Launch](resources/archives.md)

Explore Ripe's evolution from early conception through years of refinement to protocol launch.

* **Historical documentation** - Whitepapers, blog posts, and videos from 2022-2024
* **Design philosophy** - Understand the core ideas that guided development
* **Testnet learnings** - See how "Ripe Radness" seasons shaped the final protocol
* **Key refinements** - Learn why features like Juice Score evolved out

***

## The Magic: Your Risk, Your Reward, Your Portfolio

Other protocols: "Pick one: safe OR efficient."\
Ripe: "Why not both?"

* **Your collateral, your loan** — Nobody else's blowup touches your position
* **Everything counts** — Stock tokens, ETH, stablecoins, yield positions, all in one loan
* **Sleep easy** — One number to watch, and two warning zones before anything is forced

***

## ❓ Common Questions

Have questions? Check our comprehensive [**FAQ**](resources/faq.md) for answers to:

* Can I borrow against tokenized stocks without selling them?
* What happens to my stock tokens when the market is closed?
* Can I lose my stock tokens while borrowing?
* How is GREEN different from DAI or USDC?
* What happens during liquidations?
* How does sGREEN earn?
* And much more...

**→** [**Read the FAQ**](resources/faq.md)

***

## Pick Your Play

Four ways to put your assets to work. All of them better than watching them do nothing.

### 🎯 **The Stock Move**: Borrow Without Selling

Your stock tokens + your ETH + your stablecoins = one GREEN loan. Keep the upside, get the liquidity.\
**→** [**Start Borrowing**](core-protocol/02-borrowing.md)

### 🟢 **The Easy Button**: Hold sGREEN

Swap to sGREEN. Close laptop. Your balance's GREEN value climbs with protocol revenue.\
**→** [**Get sGREEN**](earning-and-rewards/01-sgreen.md)

### ⚡ **The Liquidation Game**: Buy the Dip Automatically

Someone overleveraged their stock position? The Stability Pool buys the collateral below market, and you own your share of it.\
**→** [**Join the Pools**](earning-and-rewards/02-stability-pools.md)

### 🏗️ **The Long Game**: Lock RIPE, Run the Protocol

Governance power grows with time. Lock today, steer tomorrow.\
**→** [**Lock and Lead**](governance-and-economics/02-governance.md)

***

## 🤝 Join the Ripe Community

### Connect & Learn

* **Discord**: [Join our community](https://discord.gg/hightop) — Get help, share strategies, and connect with other users
* **Twitter/X**: [@RipeProtocol](https://x.com/ripe_dao) — Latest updates and announcements
* **Blog**: [Medium](https://medium.com/ripe-finance) — Deep dives and protocol insights
* **GitHub**: [ripe-foundation](https://github.com/Ripe-Foundation/ripe-protocol) — Open source code and development

### Need Help?

* **FAQ**: Check our [Frequently Asked Questions](resources/faq.md) first
* **Discord Support**: Ask in #help channel for quick help
* **Documentation**: You're in the right place! Navigate using the menu above

***

## The Real Vision

Public equities are moving onchain. Stocks first; bonds, funds, and other real-world assets behind them. Every other lending protocol was built for crypto-native collateral and treats a tokenized share as an afterthought.

Ripe was built for this moment: a tokenized stock is as easy to borrow against as ETH, in the same loan, with the same rules. As more of the world's assets get tokenized, the same architecture takes them in.

Don't sell. Borrow. Even the ones you bought at the top. Especially those.

***

_For technical implementation details, see the_ [_Technical Documentation_](https://ripe-finance.gitbook.io/ripe-developers)_._
