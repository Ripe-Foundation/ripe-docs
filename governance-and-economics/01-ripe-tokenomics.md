---
description: Where Early Believers Win
---

# RIPE Tokenomics: Where Early Believers Win

Every DeFi token promises "fair distribution." Then VCs dump on retail.

RIPE flips the script. Community incentives fund protocol participation, while team and backer allocations follow their vesting contracts. The community allocation can supply the configured reward and bond budgets described below.

One small seed round at $0.02 after 2+ years of building. Just builders who bet their own money and users who show up early.

> **📊 Tokenomics at a Glance**
>
> * **Base allocation**: 1B RIPE across the ordinary tokenomics schedule; the token contract does not enforce a numeric supply cap
> * **Community first**: 25% of the 1B base allocation goes to user incentives; this is the only bucket that begins unlocking at TGE via block rewards & bonding
> * **Onchain schedules**: Vesting, claims, freezes, cancellations, and transfers follow the contributor contracts

For a deep dive into how RIPE powers the protocol—including [staking rewards](../earning-and-rewards/03-ripe-rewards.md), protocol fees via [sGREEN](../earning-and-rewards/01-sgreen.md), [governance participation](02-governance.md), and treasury building through [bond sales](03-bonds.md)—explore these detailed guides. Governance-vault positions accumulate points under the configured mechanism; voting availability and interfaces are deployment state.

## Token Allocation: Community-First Distribution

The 1 billion RIPE base allocation is divided across five key stakeholder groups, with the largest portion dedicated to community incentives:

![RIPE Token Allocation](https://miro.medium.com/v2/format:webp/1*2OWDZIl3gjqJl_B6JXyyaw.png)

### Community Incentives (25% - 250M RIPE)

Block rewards, bonding discounts, and LP rewards that directly incentivize protocol usage and growth. This is the only allocation that begins distributing at TGE.

### Ripe Foundation Treasury (22.2% - 222M RIPE)

Long-term liquidity provisions, strategic partnerships, ecosystem grants, and marketing initiatives to ensure protocol sustainability.

### Core Contributors (20.6% - 206M RIPE)

Compensation for 2.5 years of full-time protocol development already completed, plus ongoing development through the 4-year vesting period, ensuring the team remains committed to building and improving the protocol.

### Distribution Partner - Hightop (15% - 150M RIPE)

Strategic distribution partner behind Hightop, a digital banking product for AI agents, made simple for humans. Hightop lets users and their AI agents manage funds, earn yield, and borrow, while Ripe powers the lending layer underneath.

### Early Backers (17.2% - 172M RIPE)

Seed investors who provided capital and strategic guidance during testnet development, helping accelerate the path to mainnet launch.

## Emission Schedule: Sustainable Token Release

The graph below shows the RIPE Token emission schedule by category for years 1 to 10 following TGE.

![RIPE Emission Schedule](https://miro.medium.com/v2/format:webp/1*HQcm7_N9ZD6hbEn3-OUp9Q.png)

### Base Allocation and Authorized Minting

The token contract does not enforce a numeric maximum supply. Minting is restricted to departments authorized by RipeHQ and can be disabled by the protocol-wide minting circuit breaker. The tokenomics schedule defines a 1 billion-token base allocation. During a qualifying [RIPE Bond](03-bonds.md) purchase, the complete calculated payout must first fit the available bond allowance; if the payment clears bad debt, the corresponding part of that payout is accounted outside ordinary bond-distribution usage. That recovery accounting can expand minted supply beyond the base allocation and dilute holders.

### Vesting Schedules by Category

Community Incentives stand alone as the only allocation unlocking from day one, ensuring immediate protocol activity:

**Community Incentives (Beginning at TGE)**

* **First Unlock**: Immediate at TGE
* **Release Length**: 10+ years
* **Pattern**: Dynamic distribution via block rewards, bonding discounts, and governance

**Core Contributors (Locked First Year)**

* **First Unlock**: 12 months from TGE
* **Total Length**: 4 years
* **Release Pattern**: 25% unlocked at month 12, then linear vesting over 36 months
* **Example**: A contributor with 1M RIPE allocation receives 250K at month 12, then \~694 RIPE per day

**Ripe Foundation, Distribution Partner & Early Backers (Aligned Schedules)**

* **First Unlock**: 12 months from TGE
* **Total Length**: 3 years
* **Release Pattern**: 33% unlocked at month 12, then linear vesting over 24 months
* **Example**: An early backer with 1M RIPE receives 330K at month 12, then \~931 RIPE per day

## Early Backers: Bootstrap to Launch

### Capital Efficiency Through Self-Funding

Ripe Protocol represents a new model for DeFi development—bootstrapped primarily by its builders. Since committing [full-time](https://medium.com/hightop/hightop-sunset-ripe-sunrise-b2559ff9a7e4) to the protocol, the team has deployed $1.87M in capital:

**Core Contributor Funding**: $1.32M

* Self-funded by the founding team
* Covered operational expenses, legal structure, and security audits
* Demonstrates deep personal commitment to the protocol's success

**Seed Round**: $550K (February 2025)

* Raised via Ripe Foundation at $0.02 per RIPE
* Implied fully diluted valuation: $20M
* First external capital after 2+ years of development
* Funds ongoing operational expenses and launch preparation

### Strategic Seed Investors

The seed round brought together a carefully selected group of strategic partners who share the vision for sustainable DeFi:

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

These early supporters bring diverse perspectives from across DeFi, helping guide the protocol's development while sharing in its long-term success through aligned vesting schedules.

## How Contributor Vesting Works

### Smart Contract Automation

Contributor contracts encode their schedules and permissioned lifecycle onchain. A contract:

* **Calculates vesting** linearly from its start timestamp to its end timestamp
* **Allows cashing** vested-but-uncashed RIPE after vesting begins, even before the cliff
* **Deposits cashed RIPE** into the current core governance vault using the contract's stored duration, clamped to that vault's live minimum and maximum lock terms
* **Supports administration** through explicit freeze and paycheck-cancellation permissions

### The Vesting Process

**1. Token Release**

```
Vested Amount = Total Allocation × (Time Elapsed / Vesting Duration)
```

Tokens vest continuously by timestamp between the contract's start and end times.

**2. Claiming Tokens**

* Contributors can cash vested RIPE after vesting begins; the cliff is not a claim gate
* Claimed tokens are minted and deposited into the [Governance Vault](02-governance.md)
* Tokens remain locked and accumulate governance-vault points according to the vault's share, time, asset-weight, and remaining-lock mechanics

**3. Unlocking for Transfer**

* After the unlock period (varies by group), tokens become transferable
* Two-phase security process: initiate → wait → confirm
* The final contributor-position handoff uses the lock duration confirmed for that transfer path; it is distinct from the live-term clamp applied when a paycheck is cashed
* Protects against compromised accounts and hasty decisions

### Transparency & Security

Every vesting contract is visible onchain, allowing anyone to verify:

* Total allocation and vesting schedule
* Tokens claimed vs. remaining
* Exact unlock dates
* The configured owner, manager, and administrative lifecycle

Authorized Human Resources governance can freeze a contributor contract or cancel its paycheck before the vesting end. A freeze pauses cashing and contributor-controlled transfers. On cancellation, the cliff determines the settlement path: before the cliff, the position can be forfeited and burned; at or after the cliff, the contract first tries to cash vested RIPE and returns the remainder to the Human Resources budget. If the contract is frozen, that cash step returns zero, so vested-but-uncashed RIPE can also be forfeited. Previously cashed RIPE follows the contract's governance-vault and transfer rules.

_For deep technical details on the vesting system, see the_ [_Contributor contract documentation_](https://ripe-finance.gitbook.io/ripe-developers/treasury-and-rewards/contributor)_._

## RIPE Value Accrual: Real Revenue, Real Buybacks

Most governance tokens are worthless. Vote on stuff, hope number goes up.

The Credit Engine can allocate a configured portion of borrower interest and origination fees to the governance recipient for RIPE buyback operations. The remainder of that revenue is sent to sGREEN backing. A zero buyback ratio directs the full amount to sGREEN.

### How the Buyback Split Works

```
Protocol Revenue (Fees + Interest)
              ↓
    ┌─────────┴─────────┐
    ↓                   ↓
RIPE Buybacks     sGREEN Yield
```

Governance sets the split. The contract transfers the buyback allocation as GREEN to the governance recipient; any market purchase is a separate operation rather than an automatic Credit Engine trade.

**What happens after allocation?** Governance controls how the buyback allocation is executed and how any acquired RIPE is used; the Credit Engine itself does not enforce a purchase, distribution, or burn.

### Underscore Adds More

An [Underscore](https://underscore.finance/) strategy can have its own performance-fee and buyback policy. That external policy is separate from Ripe's Credit Engine revenue split and should be evaluated from the configured strategy.

### The Full Picture

| Revenue Source | Where It Goes |
|----------------|---------------|
| Borrowing fees (Daowry) | Configured split: governance buyback allocation + sGREEN |
| Accrued borrower interest | Configured split: governance buyback allocation + sGREEN |
| Underscore performance fees | Determined by the external strategy's policy |

The contract-enforced Ripe split applies to borrower revenue; external strategy economics remain separate.

## The Bottom Line: Own the Future, Not the Hype

RIPE isn't another VC exit scam dressed up as "community ownership."

Founders self-funded for 2.5 years. Locked for another year after launch. Early backers paid real money at a fair price. Every token unlock happens onchain where you can audit it.

But here's what matters: the community allocation is designed for users. [Block rewards](../earning-and-rewards/03-ripe-rewards.md) distribute from a funded, configured reward budget, while [bonds](03-bonds.md) draw from their separately tracked budget when that mechanism is configured and enabled.

The protocol that wins is the one that survives. The one that survives is the one people own.

Your move.
