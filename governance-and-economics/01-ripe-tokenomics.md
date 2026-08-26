---
description: Where Early Believers Win
---

# RIPE Tokenomics: Where Early Believers Win

Every DeFi token promises "fair distribution." Then VCs dump on retail.

RIPE flips the script. Community gets tokens first. Team and investors wait — and then wait a year longer than their vesting says. Early backers paid 2 cents while builders self-funded development. Now 250 million RIPE flows to the people who actually use the protocol.

One small seed round at $0.02 after 2+ years of building. Just builders who bet their own money and users who show up early.

> **Live terms live onchain.** Reward rates, bond terms, lock settings, and the revenue split vary by deployment and change over time. Allocations, dates, and the supply cap on this page are stated policy; every other number is an example. [Params](https://params.ripe.finance) is the source of truth.

**Tokenomics at a glance**

* **1 billion RIPE, total, across every chain** — bad-debt recovery counts against the cap, not on top of it
* **Community first** — 25% of supply goes to user incentives, the only bucket unlocking from day one
* **Team and investors locked longer** — an extra year on top of vesting
* **All vesting onchain** — contributor contracts you can audit today

## Token Allocation: Community-First Distribution

The 1 billion RIPE supply is split across five groups, with the largest share going to the community:

![RIPE Token Allocation](https://miro.medium.com/v2/format:webp/1*2OWDZIl3gjqJl_B6JXyyaw.png)

### Community Incentives (25% - 250M RIPE)

Block rewards, bonding discounts, and LP rewards that pay people for using the protocol. The only allocation distributing from TGE.

### Ripe Foundation Treasury (22.2% - 222M RIPE)

Long-term liquidity, strategic partnerships, ecosystem grants, and marketing.

### Core Contributors (20.6% - 206M RIPE)

Compensation for 2.5 years of full-time development already done, plus ongoing work through a 4-year vesting period.

### Distribution Partner - Hightop (15% - 150M RIPE)

Strategic distribution partner behind Hightop, a digital banking product for AI agents, made simple for humans. Hightop lets users and their AI agents manage funds, earn yield, and borrow, while Ripe powers the lending layer underneath.

### Early Backers (17.2% - 172M RIPE)

Seed investors who provided capital and strategic guidance during testnet development.

## Supply Cap: One Billion, Everywhere

RIPE is capped at 1 billion tokens, total, across every chain it lives on. One cap for the whole protocol — not one per chain.

* **Bad debt.** If liquidations ever leave the protocol with bad debt, [bonds](03-bonds.md) can clear it. The RIPE paid to those bond buyers comes out of the same 1 billion. It's tracked separately so anyone can see it, but it is not extra supply on top of the cap.
* **Bridging.** Cross-chain transfers are designed as burn-and-mint: RIPE is burned on one side and minted on the other. Supply moves; it doesn't grow.

No single token contract encodes the aggregate number. Governance administers the cap across chains, and each chain's token only mints for contracts governance has approved. [Rewards](../earning-and-rewards/03-ripe-rewards.md), [bonds](03-bonds.md), the [Reserve Engine](04-reserve-engine.md), and contributor pay each draw on their own accounting allowance rather than a pile of pre-minted tokens.

## Emission Schedule: Sustainable Token Release

The graph below shows RIPE emissions by category for years 1 to 10 after TGE. It shows the underlying category schedules; the extra one-year lock described below sits on top.

![RIPE Emission Schedule](https://miro.medium.com/v2/format:webp/1*HQcm7_N9ZD6hbEn3-OUp9Q.png)

### Vesting Schedules by Category

**Community Incentives (Unlocking Now)**

* **First unlock**: Immediate at TGE
* **Release length**: 10+ years
* **Pattern**: Dynamic — block rewards, bonds, and governance set the pace

**Core Contributors**

* **Vesting**: Linear from each contributor's start date over 4 years
* **Where it goes**: Vested RIPE is cashed into a governance-vault position (details below)

**Ripe Foundation, Distribution Partner & Early Backers**

* **Agreed schedule**: 33% releasable at month 12, then linear over the following 24 months
* **Example**: A 1M RIPE allocation releases 330K at month 12, then the remaining 670K over 730 days — about 918 RIPE a day

## Release Schedule: The Extra Year

Every investor and every employee agreed to a one-year lock on top of their vesting schedule. TGE was June 26, 2025. The earliest date any investor allocation releases or any employee position transfers is **June 26, 2027**. Vesting keeps running in the meantime — contributors can cash vested RIPE into the governance vault throughout — but nothing leaves those hands before that date.

## Early Backers: Bootstrap to Launch

### Capital Efficiency Through Self-Funding

Ripe was bootstrapped mostly by its builders. Since committing [full-time](https://medium.com/hightop/hightop-sunset-ripe-sunrise-b2559ff9a7e4) to the protocol, the team has put $1.87M to work:

**Core Contributor Funding**: $1.32M

* Self-funded by the founding team
* Covered operations, legal structure, and security audits

**Seed Round**: $550K (February 2025)

* Raised via Ripe Foundation at $0.02 per RIPE
* Implied fully diluted valuation: $20M
* First outside capital after 2+ years of development

### Strategic Seed Investors

**Institutional Partners**

* [**OrangeDAO**](https://www.orangedao.xyz/): YCombinator alumni network (also advisor allocation)
* [**Big Brain**](https://www.bigbrain.holdings/): Crypto-Native VC
* [**Tetranode**](https://x.com/Tetranode): Prominent DeFi investor/whale

**Individual Strategic Investors**

* **Sid Krommenhoek**: Partner at [Album VC](https://www.album.vc/)
* **Stephen McKeon**: Partner at [Collab+Currency](https://www.collabcurrency.com/)
* **Trevor Koverko**: Founder of [Sapien](https://www.sapien.io/)
* **Doug Leonard**: Founder of [HiFi Finance](https://hifi.finance/)
* **AJ Taylor**: Founder of [Etherfuse](https://www.etherfuse.com/)

## How Vesting Works: Onchain Contracts

Each contributor, foundation member, and investor gets a personal vesting contract. No lawyers, no spreadsheets:

```
Vested Amount = Total Allocation × (Time Elapsed / Vesting Duration)
```

* **Vesting is linear from the start date.** Every block moves the number up.
* **Cash any time.** Vested RIPE can be cashed at any point. It's minted straight into a locked position in the [governance vault](02-governance.md), where it earns governance points like any other locked RIPE. A governance freeze on the contract stops cashing, and so does a protocol-wide pause on RIPE minting.
* **The cliff only matters if the contract is cancelled.** Cancel before the cliff and the whole allocation is forfeited — the vault position included, which is burned. Cancel after it and vested RIPE is cashed out first; only the unvested remainder returns to the contributor allowance. (A frozen contract can't cash, so cancelling while frozen forfeits whatever hasn't been cashed.)
* **Transfer is a separate unlock.** Once the contract's unlock date passes, moving the vault position from the contract to the owner's own address is a two-step process: initiate, wait, confirm. The RIPE stays in the governance vault on the contract's lock terms.

Every contract is public: total allocation, schedule, claimed versus remaining, and the unlock date are all readable onchain. Governance can freeze or cancel a contract; cancellation follows the cliff rule above.

_For technical details on the vesting system, see the_ [_Contributor contract documentation_](https://ripe-finance.gitbook.io/ripe-developers/treasury/contributor)_._

## RIPE Value Accrual

Most governance tokens are worthless. Vote on stuff, hope number goes up.

RIPE sits behind real revenue. Every origination fee and every dollar of realized interest is GREEN, and governance decides how to split it:

```
Protocol Revenue (Origination Fees + Realized Interest)
              ↓
    ┌─────────┴─────────┐
    ↓                   ↓
sGREEN Yield     Governance Allocation
```

Today the split is 100% to [sGREEN](../earning-and-rewards/01-sgreen.md). Governance can move a share to its own allocation. The protocol sends that GREEN to governance; whether to buy RIPE with it is a separate governance action, made in the open.

| Revenue Source | Where It Goes |
|----------------|---------------|
| Origination fees (daowry) | Split between sGREEN and governance — all to sGREEN today |
| Realized interest | Same split |

More usage, more revenue, more flowing to holders. Simple math.

## The Bottom Line: Own the Future, Not the Hype

RIPE isn't another VC exit scam dressed up as "community ownership."

Founders self-funded for 2.5 years. Early backers paid real money at a fair price. Both agreed to an extra year on top of vesting — nothing of theirs releases before June 26, 2027. Every vesting contract is onchain where you can audit it.

But here's what matters: 250 million RIPE goes to users. Not eventually. Now. Through [rewards](../earning-and-rewards/03-ripe-rewards.md) that started at TGE. Through [bonds](03-bonds.md) and the [Reserve Engine](04-reserve-engine.md) that trade capital and patience for RIPE. Through actual usage, not Twitter campaigns.

The protocol that wins is the one that survives. The one that survives is the one people own.

Your move.
