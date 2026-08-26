---
description: One Loan. Every Asset. Maximum Power.
---

# Ripe Protocol: One Loan. Every Asset. Maximum Power.

DeFi lending works great — if you're willing to manage five different vaults. But most people aren't. Five vaults for five assets isn't how people think about their wealth. Your tokenized stocks? Worthless as collateral. Your yield-bearing tokens? Just sitting there, unused.

Ripe simplifies everything. Your entire portfolio — crypto, RWAs, jpegs, everything — backs ONE loan. Finally, borrowing that understands what a portfolio actually is.

These docs explain how Ripe's components work. Supported assets, parameters, and optional feature availability vary by deployment; use [RIPE Params](https://params.ripe.finance) for current onchain configuration.

***

## The Problem: DeFi Thinks Your Assets Live in Silos

**Isolated Vaults** (MakerDAO, Liquity) ETH in one vault. WBTC in another. Five assets? Five loans. Five liquidation risks. It's 2025 and we're still pretending portfolios don't exist.

**Shared Risk Pools** (Aave, Compound)\
Your ETH backs someone else's degen trade. Their bad debt? Your problem. Oh, and forget about using tokenized stocks or treasury bills.

**"Innovation" That Isn't** (Morpho, Euler) Slightly better rates. Same old isolation. Each market fragments liquidity further. The future looks exactly like the past.

## The Ripe Solution: Unified Multi-Collateral Lending

```
Your Portfolio:              Traditional:           Ripe:
┌─────────────┐             ┌─────────────┐       ┌─────────────┐
│ 5 ETH       │ ──────────> │ ETH Loan #1 │       │             │
│ 2 WBTC      │ ──────────> │ WBTC Loan #2│       │  ONE LOAN   │
│ 10K USDC    │ ──────────> │ USDC Loan #3│  ───> │  GREEN      │
│ RWA tokens  │ ──────────> │ Can't use   │       │  All Assets │
│ T-bills     │ ──────────> │ Can't use   │       │  Working    │
└─────────────┘             └─────────────┘       └─────────────┘
                            5 positions to          1 unified
                            manage, limited         position,
                            assets                  everything works
```

***

## Core Protocol

#### 🟢 [GREEN: The Stablecoin That Actually Does Something](core-protocol/01-green-stablecoin.md)

USDC sits there. DAI does nothing. GREEN? It's working.

* **Mint it** by borrowing against literally anything
* **Stays at $1** through five mechanisms that actually work
* **Burns on repayment** — no infinite supply games

#### 💰 [Borrowing: Your Whole Portfolio, One Loan](core-protocol/02-borrowing.md)

Stop juggling vaults. Stop wasting collateral. Just borrow.

* **Total portfolio power** — Add up everything, borrow once
* **Smart rates** — Sustained reference-pool imbalance can add a capped rate adjustment
* **Partial liquidations** — Lose some, not all

#### 🏦 [Collateral Assets: Everything Has Value](core-protocol/03-collateral-assets.md)

From stablecoins to tokenized stocks, from blue-chip crypto to your favorite PFP — if it has value, it can back your loan.

* **Unified positions** - All assets support one loan
* **Weighted terms** - Each asset contributes its best LTV
* **Extensible vaults** - Support for future asset types

#### 🛡️ [Liquidations: Protection Through Innovation](core-protocol/04-liquidations.md)

Four layers of defense. Partial liquidations only. You keep most of your collateral.

* **Redemption buffer** - GREEN holders delever you at $1 (no penalty)
* **Your assets first** - Uses eligible debt-settlement positions before ordinary collateral auctions
* **Stability pools** - Conditional settlement at configured spreads before auction fallback
* **Dutch auctions** - Time-based pricing for remaining collateral

## Earning & Rewards

#### 💎 [sGREEN: Set It and Forget It Yield](earning-and-rewards/01-sgreen.md)

GREEN that grows while you sleep. No strategies. No management. Just yield.

* **Configured revenue share** — Borrower interest and origination fees can increase GREEN backing per share
* **Zero lockup** — Your money, your timeline
* **Still liquid** — Use in stability pools while earning

#### 🎯 [Stability Pools: Buy the Dip Automatically](earning-and-rewards/02-stability-pools.md)

Forget MEV bots. Deposit, wait, profit from liquidations.

* **Configured liquidation spread** on eligible collateral routed through the pool
* **Multiple return sources** — underlying yield, liquidation results, and configured RIPE rewards
* **Zero effort** — Smart contracts do the work

#### 🪙 [RIPE Rewards: Earn by Using the Protocol](earning-and-rewards/03-ripe-rewards.md)

Earn RIPE by using the protocol. Time-weighted rewards ensure fair distribution.

* **Allowance-capped distribution** — RIPE is minted when accrued rewards are claimed
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

Lock RIPE tokens to accumulate voting power and earn protocol rewards.

* **Time multipliers** - Configured lock terms can raise the governance-point rate
* **Staker rewards** - Earn from protocol emissions
* **Future control** - Guide protocol evolution

#### 🎟️ [Bonds: Fund the Future](governance-and-economics/03-bonds.md)

Exchange a configured payment asset for RIPE at an epoch price, supporting protocol growth.

* **Unlocked or locked** - A qualifying governance-vault lock can add a configured bonus
* **Bond Boosters** - Bonus rewards for ecosystem contributors
* **Permanent funding** - Builds the Endaoment treasury

#### 🏦 [RIPE Reserve Engine: Acquire RIPE with Vesting](governance-and-economics/04-reserve-engine.md)

Exchange a configured payment token for a RIPE allocation that vests over time.

* **Epoch pricing** - Terms are fixed within each committed epoch
* **Duration bonus** - Longer vesting can increase the allocation
* **Mint on claim** - Payment is collected up front; RIPE is minted as it vests and is claimed

#### 🏰 [The Endaoment: Protocol Treasury](core-protocol/07-endaoment.md)

Protocol-owned treasury for configured yield, liquidity, and peg-support operations.

* **Multi-strategy yields** - Optimized across DeFi
* **Peg defense** - Authorized market operations
* **Partner programs** - Collaborative liquidity

## Resources

#### 🔮 [Price Oracles: Accurate Asset Valuation](core-protocol/06-price-oracles.md)

Multi-oracle system ensuring fair and manipulation-resistant pricing.

* **Configurable source adapters** - External feeds, market pools, and derived-asset pricing
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

* **Your collateral, your loan** — No sharing risk with degens
* **Everything counts** — From ETH to tokenized gold to treasury bills
* **Any asset works** — If it has value, you can borrow against it
* **Sleep easy** — Your position doesn't care what others do

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

Four ways to win. All of them better than watching your assets do nothing.

### 🟢 **The Easy Button**: Buy sGREEN, Earn Forever

Swap to sGREEN. Close laptop. Come back richer. That's it.\
**→** [**Get sGREEN Now**](earning-and-rewards/01-sgreen.md)

### ⚡ **The Liquidation Game**: Buy ETH at 90 Cents on the Dollar

Someone else overleveraged? Their loss, your gain. Stability pools turn liquidations into profit.\
**→** [**Join the Pools**](earning-and-rewards/02-stability-pools.md)

### 🎯 **The Power Move**: One Loan to Rule Them All

Your tokenized Tesla shares + ETH + treasury bills + that yield position = One massive loan. Finally.\
**→** [**Start Borrowing**](core-protocol/02-borrowing.md)

### 🏗️ **The Long Game**: Lock RIPE, Run the Protocol

Governance power grows with time. Lock today, control tomorrow.\
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

## The Real Vision: Unlocking $16 Trillion

Here's what matters: By 2030, $16 trillion in real-world assets will be tokenized. Stocks. Real estate. Gold. Treasury bills.

Traditional DeFi can't handle them. Too risky. Too different. Too complicated.

Ripe was built for this moment. When your tokenized Tesla shares are as easy to borrow against as your ETH. When that tokenized apartment in São Paulo unlocks liquidity for its owner. When a farmer in Guatemala can finally access capital using tokenized land.

Every other protocol is fighting over the same $500B in crypto. We're building for the $16 trillion that's coming.

One loan. Every asset. The future of finance.

Welcome to lending that actually makes sense.

***

_For technical implementation details, see the_ [_Technical Documentation_](https://ripe-finance.gitbook.io/ripe-developers)_._
