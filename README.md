---
description: One Loan. Supported Assets. Portfolio Power.
---

# Ripe Protocol: One Loan. Supported Assets. Portfolio Power.

DeFi lending works great — if you're willing to manage five different vaults. But most people aren't. Five vaults for five assets isn't how people think about their wealth. Your tokenized stocks? Worthless as collateral. Your yield-bearing tokens? Just sitting there, unused.

Ripe combines supported collateral into ONE loan. Finally, borrowing that understands what a portfolio actually is.

These docs explain Ripe's mechanisms and use illustrative examples. Supported assets, parameters, oracle routes, and optional feature availability can vary by network and governance configuration; see [RIPE Params](https://params.ripe.finance) for current onchain settings.

***

## The Problem: DeFi Thinks Your Assets Live in Silos

**Isolated Vaults** (MakerDAO, Liquity) ETH in one vault. WBTC in another. Five assets? Five loans. Five liquidation risks. Portfolios still get split into separate positions.

**Shared Risk Pools** (Aave, Compound)\
Your ETH backs someone else's trade. Their bad debt can affect the shared market. Collateral support still depends on the assets and risk terms configured for that market.

**"Innovation" That Isn't** (Morpho, Euler) Slightly better rates. Same old isolation. Each market fragments liquidity further. The future looks exactly like the past.

## The Ripe Solution: Unified Multi-Collateral Lending

```
Illustrative Portfolio:      Traditional:           Ripe:
┌─────────────┐             ┌─────────────┐       ┌─────────────┐
│ 5 WETH      │ ──────────> │ WETH Loan #1│       │             │
│ 2 WBTC      │ ──────────> │ WBTC Loan #2│       │  ONE LOAN   │
│ 10K USDC    │ ──────────> │ USDC Loan #3│  ───> │  GREEN      │
│ Asset C     │ ──────────> │ Loan #4     │       │  Supported  │
│ Asset D     │ ──────────> │ Loan #5     │       │  Assets     │
└─────────────┘             └─────────────┘       └─────────────┘
                            5 positions to          1 unified
                            manage, limited         position,
                            assets                  together
```

***

## Core Protocol

#### 🟢 [GREEN: The Stablecoin That Actually Does Something](core-protocol/01-green-stablecoin.md)

USDC sits there. DAI does nothing. GREEN? It's working.

* **Mint it** by borrowing against supported collateral
* **Targets $1** through complementary stability mechanisms
* **Standard GREEN repayments burn the payment**; separate configured settlement paths can reduce debt without a GREEN burn

#### 💰 [Borrowing: Your Whole Portfolio, One Loan](core-protocol/02-borrowing.md)

Stop juggling vaults. Stop wasting collateral. Just borrow.

* **Total portfolio power** — Combine supported collateral, borrow once
* **Smart rates** — A sustained reference-pool signal can add a capped dynamic adjustment
* **Target-based liquidations** — Sell only what is needed when possible

#### 🏦 [Collateral Assets: Portfolio-Based Borrowing](core-protocol/03-collateral-assets.md)

The vault architecture can support different token and accounting models when governance configures compatible custody and pricing.

* **Unified positions** - Supported collateral contributes to one loan
* **Weighted terms** - Each asset contributes its configured debt terms
* **Extensible vaults** - Support for future asset types

#### 🛡️ [Liquidations: Protection Through Innovation](core-protocol/04-liquidations.md)

Ripe uses separate deleverage, redemption, Stability Pool, and auction paths to reduce unsafe debt. A liquidation targets restored health and may be partial, but severe shortfalls can require all eligible collateral.

* **Redemption buffer** - GREEN can be exchanged for eligible collateral through the separate configured redemption path
* **Separate deleverage path** - Eligible GREEN-side assets can burn and configured stable-side assets can transfer for debt credit
* **Stability pools** - Conditional settlement before ordinary auctions
* **Dutch auctions** - Time-based pricing for remaining collateral

## Earning & Rewards

#### 💎 [sGREEN: Share-Based GREEN Savings](earning-and-rewards/01-sgreen.md)

Deposit GREEN into a non-rebasing savings share. Its backing per share can increase when configured protocol revenue reaches the vault.

* **Configured revenue share** — Captures the portion of borrower interest and fees directed to sGREEN
* **No time lock** — Exits remain subject to token and vault safety controls
* **Stability-compatible** — Can provide settlement liquidity while retaining sGREEN economics

#### 🎯 [Stability Pools: Buy the Dip Automatically](earning-and-rewards/02-stability-pools.md)

Deposit to gain passive exposure to configured liquidation spreads and claim assets.

* **Discounted collateral exposure** when eligible liquidations use the pool
* **Multiple return sources** — underlying yield, liquidation results, and configured RIPE rewards
* **Passive settlement exposure** — Depositors do not submit each liquidation themselves

#### 🪙 [RIPE Rewards: Earn by Using the Protocol](earning-and-rewards/03-ripe-rewards.md)

Configured activities can accrue RIPE entitlement through elapsed-point accounting.

* **Accounting allowance** - Accrual is capped by the remaining configured reward allowance; RIPE is minted on claim
* **Configurable categories** - Borrower, staker, depositor, and voter allocations
* **Auto-staking** - A configured claim portion can deposit into governance

## Governance & Economics

#### 📊 [RIPE Tokenomics: Aligned Incentives](governance-and-economics/01-ripe-tokenomics.md)

Understanding RIPE's 1 billion-token base allocation and the vesting schedules that power the protocol.

* **Base allocation** - 1B ordinary allocation; authorized minting and bad-debt bond accounting are explained in the tokenomics guide
* **Community first** - 25% for rewards, largest unlocking allocation
* **Transparent vesting** - All schedules enforced onchain
* **Strategic funding** - $1.87M of founder and external capital described in the tokenomics guide

#### 🏛️ [Governance Points and Locking](governance-and-economics/02-governance.md)

Lock RIPE tokens to accumulate governance points and, when configured, protocol rewards.

* **Time multipliers** - Configured remaining-lock terms can raise the point rate
* **Staker rewards** - Earn from protocol emissions
* **Governance integration** - Compatible governance interfaces can use accumulated points as voting weight

#### 🎟️ [Bonds: Fund the Future](governance-and-economics/03-bonds.md)

Exchange a configured payment asset for RIPE at an epoch price, supporting protocol growth.

* **Instant or locked** - A qualifying lock can add a configured bonus
* **Bond Boosters** - Bonus rewards for ecosystem contributors
* **Treasury assets** - Accepted payments become Endaoment treasury assets

#### 🏦 [RIPE Reserve Engine: Acquire RIPE with Vesting](governance-and-economics/04-reserve-engine.md)

Exchange a configured payment token for a RIPE allocation that vests over time.

* **Epoch controller** - Base payout rates can respond between committed epochs
* **Duration bonus** - A longer vesting selection can increase the allocation
* **Mint on claim** - Payment is collected at acquisition; vested RIPE is minted only when claimed

#### 🏰 [The Endaoment: Onchain Treasury](core-protocol/07-endaoment.md)

Governed treasury infrastructure for supporting GREEN and deploying protocol-owned assets.

* **Multi-strategy capability** - Authorized actions can use configured integrations
* **Peg defense** - Authorized, bounded market operations
* **Partner programs** - Collaborative liquidity

## Resources

#### 🔮 [Price Oracles: Accurate Asset Valuation](core-protocol/06-price-oracles.md)

Ordered price-source routing with isolated fallbacks and fail-closed valuation.

* **Multiple source types** - External feeds, market pools, and derived-asset pricing
* **Ordered fallbacks** - Uses the first configured source that returns a valid price
* **Fail-closed protection** - Risk-increasing actions stop when no usable price exists

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

* **Account-level position** — Your collateral and debt are accounted per account rather than funded by a pooled lender balance sheet
* **Portfolio accounting** — Supported assets contribute according to their own terms
* **Extensible vaults** — New asset types can use compatible custody and pricing modules
* **Shared protocol conditions** — Global limits, rates, oracle and liquidity conditions, and bad debt can still affect individual accounts

***

## ❓ Common Questions

Have questions? Check our comprehensive [**FAQ**](resources/faq.md) for answers to:

* How is GREEN different from DAI or USDC?
* What happens during liquidations?
* How are RIPE rewards calculated?
* What makes Ripe's multi-collateral system unique?
* How does sGREEN generate yield?
* And much more...

**→** [**Read the FAQ**](resources/faq.md)

***

## Pick Your Play

Several ways to use the protocol, each with its own conditions and risks.

### 🟢 **The Savings Path**: Convert GREEN to sGREEN

Convert GREEN to sGREEN to receive the borrower revenue directed to the savings vault through its exchange rate.\
**→** [**Get sGREEN Now**](earning-and-rewards/01-sgreen.md)

### ⚡ **The Stability Path**: Provide Liquidation Liquidity

Eligible Stability settlement exchanges vault liquidity for claimable collateral at a configured spread, with price and liquidity risk.\
**→** [**Join the Pools**](earning-and-rewards/02-stability-pools.md)

### 🎯 **The Power Move**: One Loan to Rule Them All

Combine supported collateral under its individual terms in one unified GREEN loan.\
**→** [**Start Borrowing**](core-protocol/02-borrowing.md)

### 🏗️ **The Long Game**: Lock RIPE and Stack Points

Governance points accumulate with position size and elapsed blocks; compatible governance interfaces can use them as voting weight.\
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

## The Real Vision: Extensible Portfolio Lending

More forms of value are moving onchain: stocks, real estate, commodities, treasury instruments, and new digital assets.

Traditional DeFi can't handle them. Too risky. Too different. Too complicated.

Ripe's vault architecture was built so additional asset types can join a unified loan after governance configures compatible custody, pricing, permissions, and risk terms.

One loan. Supported assets working together. Extensible by design.

Welcome to lending that actually makes sense.

***

_For technical implementation details, see the_ [_Technical Documentation_](https://ripe-finance.gitbook.io/ripe-developers)_._
