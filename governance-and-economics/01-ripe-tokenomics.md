---
description: Protocol-wide supply, allocation, and distribution mechanics
---

# RIPE Tokenomics

RIPE has a maximum supply of 1 billion tokens in aggregate across all blockchains. This is one protocol-wide cap, not a separate 1 billion-token cap for each network.

The cap must be administered across authorized issuance and cross-chain operations. Individual deployments track local supply and mint authority; no individual deployment independently maintains or enforces the cross-chain aggregate.

RIPE distribution combines several mechanisms. Community incentives support protocol participation, while contributor and backer positions follow their vesting and release terms. Rewards, bonds, and the Reserve Engine consume separate configured accounting allowances rather than a pre-minted pool held in escrow. The protocol-wide cap, allocation percentages, and historical financing figures below are Ripe Foundation policy and business disclosures, documented here as of August 2026. The investor and employee extension is an additional holder commitment layered on those published schedules.

> **📊 Tokenomics at a Glance**
>
> * **Protocol-wide cap**: 1B RIPE in aggregate across all blockchains, not 1B per chain
> * **Community first**: 25% of the total allocation goes to user incentives; this is the only bucket that begins unlocking at TGE via block rewards and bonding
> * **Extended alignment**: Every investor and employee in the Early Backer and Core Contributor allocations agreed to an additional one-year lock; the earliest scheduled investor release or employee position-transfer eligibility is June 26, 2027
> * **Release mechanics**: Onchain vesting mechanics and applicable holder commitments govern release timing

For a deep dive into how RIPE powers the protocol—including [staking rewards](../earning-and-rewards/03-ripe-rewards.md), protocol fees via [sGREEN](../earning-and-rewards/01-sgreen.md), [governance participation](02-governance.md), and treasury building through [bond sales](03-bonds.md)—explore these detailed guides. Governance-vault positions accumulate points under the configured mechanism; voting availability and interfaces are deployment state.

## Protocol-Wide Supply Allocation

The protocol-wide 1 billion RIPE supply is allocated across five key stakeholder groups, with the largest portion dedicated to community incentives:

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

## Release Schedule and Additional One-Year Lock

Community incentives can begin distributing at TGE. Every investor and employee in the Early Backer and Core Contributor allocations has agreed to extend their lock by one additional year. The earliest scheduled investor release or employee position-transfer eligibility under those commitments is **June 26, 2027**, two years after the **June 26, 2025** TGE. Later releases remain governed by the applicable holder schedules.

Here, the June 26 date is the first release or position-transfer eligibility date under those voluntary commitments. Vesting or accounting before that date does not mean the tokens are transferable or circulating. A contributor position transferred on or after that date remains subject to the separate governance-vault lock applied at confirmation.

### Cross-Chain Supply Cap and Authorized Minting

The 1 billion-token cap applies to total RIPE across all blockchains. Each chain's token contract tracks its local supply, and that deployment's RipeHQ controls local mint authority rather than each network receiving its own 1 billion-token allotment. Authorized issuance must therefore be administered against the single protocol-wide cap.

On a configured CCIP route, RIPE is burned on the source chain and the corresponding amount is minted on the destination after the cross-chain message is validated. A completed transfer moves RIPE between chains rather than adding a second supply allocation.

Rewards mint RIPE when a claim consumes reward entitlement, and [Ripe Bonds](03-bonds.md) mint RIPE when a purchase consumes bond allowance. During a qualifying bond purchase, the complete calculated payout must first fit that allowance. Because a bad-debt-attributed payout is recorded outside ordinary bond-distribution usage, authorized cap administration must count it separately toward RIPE's protocol-wide 1 billion-token cap across all blockchains.

The [RIPE Reserve Engine](04-reserve-engine.md) has a separate Vesting allocation budget. A successful acquisition reserves its full allocation from that budget, and RIPE is minted only as vested amounts are claimed. These accounting allowances govern their respective distribution mechanisms; they are not token custody or a cross-chain cap. Authorized administration must coordinate them within the protocol-wide 1 billion-token cap. Every department-authorized post-deployment mint remains subject to that deployment's minting circuit breaker.

### Vesting Schedules by Category

Community Incentives stand alone as the only allocation unlocking from day one, ensuring immediate protocol activity:

**Community Incentives (Beginning at TGE)**

* **First distribution eligibility**: Immediate at TGE
* **Release Length**: 10+ years
* **Pattern**: Dynamic distribution via block rewards, bonding discounts, and governance

**Core Contributors (Employee Lock Extension)**

* **Vesting**: Linear from the schedule start over 4 years
* **Voluntary extension**: Every employee in this allocation agreed to an additional one-year lock
* **Earliest position-transfer eligibility**: June 26, 2027, two years after TGE
* **Before transfer eligibility**: Vested RIPE can be accounted for or cashed into a configured governance-vault position, but that does not make it transferable
* **Transfer process**: A two-step handoff can begin at eligibility; confirmation applies the Contributor contract's stored lock term to the recipient's governance-vault position, where it is share-weighted with any existing position rather than becoming immediately withdrawable

**Early Backers (Additional One-Year Lock)**

* **Voluntary extension**: All investors agreed to an additional one-year lock
* **Earliest scheduled release**: June 26, 2027, two years after TGE
* **Later releases**: Continue under the applicable holder schedules

**Ripe Foundation & Distribution Partner**

* **Published allocation schedule**: Linear from the schedule start over 3 years
* **First position-transfer eligibility**: 12 months from TGE
* **At first transfer eligibility**: Approximately one third is cumulatively vested under this schedule; vesting continues linearly afterward
* **Before transfer eligibility**: Vested RIPE can already be cashed into the configured governance vault, subject to the same distinction between cashing and transferring

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

* **OrangeDAO**: Y Combinator alumni network (also advisor allocation)
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

* After the unlock period (varies by group), a contributor-position transfer can be initiated
* Two-phase security process: initiate → wait → confirm
* The final contributor-position handoff applies the Contributor contract's stored lock duration; it is distinct from the live-term clamp applied when a paycheck is cashed
* Protects against compromised accounts and hasty decisions

### Transparency & Security

Every vesting contract is visible onchain, allowing anyone to verify:

* Total allocation and vesting schedule
* Tokens claimed vs. remaining
* Exact unlock dates
* The configured owner, manager, and administrative lifecycle

Authorized Human Resources governance can freeze a contributor contract or cancel its paycheck before the vesting end. A freeze pauses cashing and contributor-controlled transfers. On cancellation, the cliff determines the settlement path: before the cliff, the position can be forfeited and burned; at or after the cliff, the contract first tries to cash vested RIPE and returns the remainder to the Human Resources budget. If the contract is frozen, that cash step returns zero, so vested-but-uncashed RIPE can also be forfeited. Previously cashed RIPE follows the contract's governance-vault and transfer rules.

The cancellation `cliffTime` and transfer `unlockTime` are distinct contract terms even when a particular schedule assigns them the same timestamp. The cliff selects the paycheck-cancellation treatment; the unlock controls when a contributor-position transfer can be initiated.

## Borrower Revenue Allocation

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

## Policy and Contract Mechanics

The published community allocation is intended for protocol-user incentives. [Block rewards](../earning-and-rewards/03-ripe-rewards.md) consume a configured reward allowance, while [bonds](03-bonds.md) draw from their separately tracked allowance. Each mechanism mints RIPE only when its claim or purchase completes, subject to protocol mint controls.

Contributor and backer contracts expose their individual schedules and lifecycle actions onchain. Because supply and mint authority are chain-local, no individual deployment independently maintains the cross-chain total. Authorized issuance, distribution allowances, and bridge administration must operate within the single protocol-wide cap.
