---
description: Borrow GREEN against supported tokenized stocks and other eligible collateral in one unified position.
---

# Ripe Protocol: Borrow Against Tokenized Stocks and Your Portfolio

Public equity exposure is moving onchain through a growing category of tokenized stocks, often called stock tokens. Issuers use different product names — including Robinhood Stock Tokens and Coinbase Tokenized Stocks — but Ripe is built to let supported products in that category back GREEN alongside the rest of a portfolio.

Ripe makes that exposure useful: deposit supported stock tokens, borrow GREEN without selling them at origination, and keep their economic exposure while the tokens remain directly held or credited to a custody-backed vault balance.

Supported stock tokens can join other eligible collateral in ONE unified loan. Redemption, deleverage, or liquidation can transfer collateral and reduce or eliminate that exposure, so position health still matters.

These docs explain how Ripe's components work. Supported assets, parameters, and optional feature availability vary by deployment; use [RIPE Params](https://params.ripe.finance) for current onchain configuration.

***

## The Problem: DeFi Thinks Your Assets Live in Silos

**Isolated Vaults** (MakerDAO, Liquity) One asset in one vault. Another asset in another. Five assets? Five loans. Five liquidation risks. We're still pretending portfolios don't exist.

**Shared Risk Pools** (Aave, Compound)\
Your collateral backs someone else's degen trade. Their bad debt? Your problem. Oh, and forget about using Stock Tokens alongside the rest of your portfolio.

**"Innovation" That Isn't** (Morpho, Euler) Slightly better rates. Same old isolation. Each market fragments liquidity further. The future looks exactly like the past.

## The Ripe Solution: Unified Multi-Collateral Lending

```
Your Supported Collateral:  Traditional:           Ripe:
┌──────────────┐            ┌──────────────┐      ┌─────────────┐
│ Stock Tokens │ ─────────> │ Stock loan   │      │             │
│ ETH          │ ─────────> │ ETH loan     │      │  ONE LOAN   │
│ Stablecoins  │ ─────────> │ Stable loan  │ ───> │  GREEN      │
│ Yield tokens │ ─────────> │ Yield loan   │      │  Eligible   │
│ Other assets │ ─────────> │ Other loan   │      │  Collateral │
└──────────────┘            └──────────────┘      └─────────────┘
                            Separate positions     1 unified
                            to manage              position
```

***

## Core Protocol

#### 🟢 [GREEN: The Stablecoin That Actually Does Something](core-protocol/01-green-stablecoin.md)

USDC sits there. DAI does nothing. GREEN? It's working.

* **Mint it** by borrowing against supported collateral configured with borrowing power
* **Targets $1** — Six complementary mechanisms can support the peg, but none guarantees GREEN's market price
* **Burns on repayment** — no infinite supply games

#### 💰 [Borrowing Against Tokenized Stocks and Your Portfolio](core-protocol/02-borrowing.md)

Stop juggling vaults. Stop wasting collateral. Just borrow.

* **Eligible portfolio power** — Combine supported non-Stability collateral, borrow once
* **Smart rates** — Sustained reference-pool imbalance can add a capped rate adjustment
* **Targeted liquidations** — Limited shortfalls may produce a partial target; severe shortfalls can produce a full-debt target and exhaust eligible collateral without fully clearing the debt

#### 🏦 [Tokenized Stocks and Other Collateral](core-protocol/03-collateral-assets.md)

Supported tokenized stocks can back GREEN alongside other eligible collateral, all in one position.

* **Borrowing against stock tokens** - Access GREEN without selling supported stock tokens at origination
* **Unified positions** - Supported tokenized stocks and other eligible assets can back one loan
* **Weighted terms** - Each asset's borrowing power contributes to the position's weighted terms
* **Extensible vaults** - Support for future asset types

#### 🛡️ [Liquidations: Protection Through Innovation](core-protocol/04-liquidations.md)

Separate deleverage and redemption paths can reduce risk before liquidation. Liquidation itself targets safer account health through configured Stability settlement and auctions, but severe shortfalls can consume all eligible collateral and still leave debt.

* **Redemption buffer** - GREEN is treated as a $1 debt-value input when eligible collateral is sized and credited
* **Separate deleverage route** - Can use eligible debt-settlement positions before liquidation
* **Stability pools** - Conditional settlement at configured spreads before auction fallback
* **Dutch auctions** - Time-based pricing for remaining collateral

## Earning & Rewards

#### 💎 [sGREEN: Share in Configured Protocol Revenue](earning-and-rewards/01-sgreen.md)

Hold sGREEN for exposure to changes in GREEN backing per share. Outcomes depend on configured revenue flows and protocol conditions.

* **Configured revenue share** — Borrower interest and origination fees can increase GREEN backing per share
* **No fixed lock term** — Withdrawals remain subject to the vault's ordinary controls
* **Stability-compatible when configured** — Eligible sGREEN can fund liquidation settlement; consumed sGREEN is redeemed to GREEN and burned as the cohort receives collateral custody

#### 🎯 [Stability Pools: Tokenized Stock and Collateral Settlement](earning-and-rewards/02-stability-pools.md)

Deposit eligible liquidity into a shared vault. When configured Stock Tokens or other collateral settle through a compatible, funded pool, the cohort receives collateral while supplying settlement value at the effective spread. Depositors participate proportionally through vault shares; a positive return is not guaranteed.

* **Configured liquidation spread** on eligible collateral routed through the pool
* **Potential return sources** — deposited-asset economics, liquidation results, and configured RIPE rewards
* **Protocol-directed settlement participation** — An eligible submitted liquidation can route through the pool when its configuration and capacity checks pass

#### 🪙 [RIPE Rewards: Configured Rewards for Protocol Use](earning-and-rewards/03-ripe-rewards.md)

Eligible activity can accrue RIPE when its reward category, rate, and allowance are configured.

* **Allowance-capped participant distribution** — Ordinary participant RIPE is minted when accrued rewards are claimed
* **Configurable categories** - Borrower, staker, depositor, and voter allocations
* **Auto-staking** - A configured share of a claim can be deposited into governance

## Governance & Economics

#### 📊 [RIPE Tokenomics: Aligned Incentives](governance-and-economics/01-ripe-tokenomics.md)

Understanding RIPE's 1 billion-token protocol-wide supply cap and the schedules that govern distribution.

* **Cross-chain cap** - 1B RIPE in aggregate across all blockchains
* **Community first** - 25% for rewards, largest unlocking allocation
* **Extended alignment** - Investors and employees agreed to an additional one-year lock, with the earliest scheduled investor release or employee position-transfer eligibility on June 26, 2027
* **Transparent vesting** - Onchain schedules and applicable holder commitments govern release timing
* **Strategic funding** - Bootstrapped with only $1.87M raised

#### 🏛️ [Governance: Power Through Time](governance-and-economics/02-governance.md)

Lock RIPE tokens to accumulate voting power and become eligible for configured protocol rewards.

* **Time multipliers** - Configured lock terms can raise the governance-point rate
* **Staker rewards** - Can accrue from configured protocol emissions
* **Future control** - Guide protocol evolution

#### 🎟️ [Bonds: Fund the Future](governance-and-economics/03-bonds.md)

Exchange a configured payment asset for RIPE at an epoch price, supporting protocol growth.

* **Unlocked or locked** - A qualifying governance-vault lock can add a configured bonus
* **Bond Boosters** - Bonus rewards for ecosystem contributors
* **Treasury funding** - Routes accepted payment assets to EndaomentFunds for later authorized use

#### 🏦 [RIPE Reserve Engine: Acquire RIPE with Vesting](governance-and-economics/04-reserve-engine.md)

Exchange a configured payment token for a RIPE allocation that vests over time.

* **Epoch pricing** - Terms are fixed within each committed epoch
* **Duration bonus** - Longer vesting can increase the allocation
* **Mint on claim** - Payment is collected up front; RIPE is minted as it vests and is claimed

#### 🏰 [The Endaoment: Protocol Treasury](core-protocol/07-endaoment.md)

Protocol-owned treasury for configured yield, liquidity, and peg-support operations.

* **Configured strategies** - Authorized treasury operations can deploy assets for potential yield
* **Peg defense** - Authorized market operations
* **Partner programs** - Collaborative liquidity

## Resources

#### 🔮 [Price Oracles: Accurate Asset Valuation](core-protocol/06-price-oracles.md)

Ordered, fail-closed asset pricing with configurable fallback.

* **Configurable source adapters** - External feeds, market pools, and derived-asset pricing
* **Ordered fallbacks** - Uses the first configured source that returns a valid price
* **Freshness checks** - Adapter-specific validity and freshness rules determine whether a source is usable

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

* **Your collateral, your loan** — No sharing risk with degens
* **Eligible collateral counts** — From ETH to tokenized gold to treasury bills when supported
* **Supported assets work together** — Assets with configured borrowing power back one loan
* **Sleep easy** — Your position doesn't care what others do

***

## ❓ Common Questions

Have questions? Check our comprehensive [**FAQ**](resources/faq.md) for answers to:

* Can I borrow against tokenized stocks without selling them?
* What happens to Stock Tokens when their reference markets are closed?
* Can redemption or liquidation transfer my Stock Tokens?
* How is GREEN different from DAI or USDC?
* What happens during liquidations?
* How does sGREEN generate yield?
* And much more...

**→** [**Read the FAQ**](resources/faq.md)

***

## Pick Your Play

Four ways to put supported assets to work, each with its own conditions and risks.

### 🟢 **The Savings Route**: Hold sGREEN

Hold sGREEN for exposure to configured protocol revenue through its backing-per-share accounting. Returns depend on protocol activity and are not guaranteed.\
**→** [**Get sGREEN Now**](earning-and-rewards/01-sgreen.md)

### ⚡ **The Liquidation Settlement Route**: Use a Stability Pool

When configured tokenized-stock collateral settles through a compatible, funded Stability pool, the cohort receives the tokens while supplying settlement value at the effective spread. Depositors participate proportionally through vault shares, and active collateral claims enter NAV at usable oracle value. The realized outcome depends on asset prices, claim timing, and other pool conditions.\
**→** [**Join the Pools**](earning-and-rewards/02-stability-pools.md)

### 🎯 **The Stock Move**: Borrow Without Selling at Origination

Supported Stock Tokens + other eligible collateral = one unified GREEN loan. You retain the tokens' economic exposure while they remain directly held or credited to a custody-backed vault balance; redemption, deleverage, or liquidation can transfer collateral.\
**→** [**Start Borrowing**](core-protocol/02-borrowing.md)

### 🏗️ **The Long Game**: Lock RIPE, Run the Protocol

Eligible locks can increase governance-point accrual; how those points translate into authority depends on the configured governance integration.\
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

## The Real Vision: Put Tokenized Value to Work

Bringing public equity exposure onchain is only the first step. Ripe is built so supported tokenized stocks can do more than sit in a wallet: they can back GREEN liquidity alongside other eligible collateral.

Tokenized stocks are the focus. The same extensible architecture can support other tokenized real-world assets as compatible assets, pricing, and risk controls emerge.

One position. Supported Stock Tokens and other eligible collateral. Liquidity without an origination sale.

Welcome to lending that actually makes sense.

***

_For technical implementation details, see the_ [_Technical Documentation_](https://ripe-finance.gitbook.io/ripe-developers)_._
