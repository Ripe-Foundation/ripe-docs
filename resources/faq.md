---
description: Quick answers to what actually matters.
---

# Frequently Asked Questions

## Getting Started

### What is Ripe Protocol?

One loan backed by your supported collateral. While other protocols make you juggle five vaults for five assets, Ripe combines configured collateral types into ONE position. Borrow GREEN stablecoins against it all.

### What makes Ripe different from other lending protocols?

**One loan, many supported assets.** While other protocols force you to manage separate positions for each collateral type, Ripe lets you deposit multiple configured assets that all work together to back a single GREEN [borrowing position](../core-protocol/02-borrowing.md). [RIPE Params](https://params.ripe.finance) shows what is currently configured.

### What is GREEN?

[GREEN](../core-protocol/01-green-stablecoin.md) is Ripe's overcollateralized stablecoin, designed to target $1. You borrow it against supported collateral. Standard GREEN repayment burns the GREEN applied to debt; configured deleverage and Stability paths can settle debt differently.

### Is my money safe?

Your [deposits](../core-protocol/03-collateral-assets.md) back only your own loan, not other users' positions. If another borrower gets liquidated or creates bad debt, it doesn't directly affect your collateral.

## Borrowing Basics

### How much can I borrow?

Each supported collateral asset has configured Loan-to-Value (LTV) terms. Your total [borrowing power](../core-protocol/02-borrowing.md) combines the value of your deposits using those individual terms. See [RIPE Params](https://params.ripe.finance) for the current assets and LTVs on each network.

### How do interest rates work?

You pay a single weighted rate based on your [collateral mix](../core-protocol/03-collateral-assets.md), plus any applicable dynamic adjustment when the debt terms refresh.

### What are dynamic rates?

Dynamic rates can add a capped adjustment when corroborated reference-pool observations show a sustained GREEN imbalance; one isolated reading is not enough. See [Dynamic Interest Rates](../core-protocol/02-borrowing.md#dynamic-interest-rates) for the mechanism and [RIPE Params](https://params.ripe.finance) for current settings.

### Can I repay anytime?

Yes! There are no prepayment penalties, fixed terms, or lockups. Repay any amount at any time to reduce your debt and improve your position health.

## Managing Risk

### When do I get liquidated?

A position becomes eligible for [liquidation](../core-protocol/04-liquidations.md) when its collateral value reaches or falls below the required minimum for its debt. A permissionless caller must then submit an eligible liquidation transaction. There are three key thresholds to monitor (values vary by asset risk):

1. **Max LTV**: Your borrowing limit; you cannot increase debt beyond it
2. **Redemption threshold**: The zone in which GREEN holders may redeem against eligible collateral
3. **Liquidation threshold**: The point at which a liquidation episode may begin

**Illustrative example**: With $6,000 debt and an assumed 90% liquidation threshold, the position needs more than approximately $6,667 of collateral ($6,000 ÷ 0.90) to remain outside liquidation eligibility under the example terms. At or below the exact threshold, it is eligible.

Monitor your position and add collateral or repay debt before reaching these zones. For a visual guide showing all risk zones, see [How Thresholds Work Together](../core-protocol/02-borrowing.md#how-thresholds-work-together-a-visual-guide).

### What happens if one of my collateral assets cannot be priced?

If debt-bearing collateral has no usable price or backing, the account enters valuation quarantine: new borrowing and debt-supporting withdrawals stop, and new liquidation, redemption, and deleverage passes wait for valuation to recover. Repayment remains available under its normal controls. Quarantine is not itself a liquidation or insolvency finding; see [Price Oracles](../core-protocol/06-price-oracles.md#staleness-protection).

### What's the difference between redemption and liquidation?

**Redemption** is a separate path in which GREEN can be exchanged for eligible collateral using a $1 debt-value input. Crossing the liquidation threshold does not itself stop redemption; it remains available until the account enters `inLiquidation`, subject to the normal eligibility and valuation checks.

**Liquidation** becomes available at the liquidation threshold, includes configured fees, and can use Stability settlement or auctions. See [Liquidations](../core-protocol/04-liquidations.md).

### How does partial liquidation work?

Ripe targets the debt reduction needed to restore safer account health. A limited shortfall can produce a partial liquidation, while severe undercollateralization can produce a full-debt target and exhaust all eligible collateral without fully clearing the debt.

## Earning with Ripe

### What is sGREEN?

[sGREEN](../earning-and-rewards/01-sgreen.md) is yield-bearing GREEN whose backing can increase from the configured share of realized protocol revenue. Your sGREEN balance stays the same while its GREEN value reflects backing per share through:

* Borrower interest payments
* Origination fees from new loans
* Protocol revenue distributions

No staking or claiming needed — just hold and earn.

### How do stability pools work?

[Stability pools](../earning-and-rewards/02-stability-pools.md) hold configured settlement assets. Compatible liquidations can exchange available vault liquidity for claimable collateral at a configured spread. Depositors retain the economics and risks of the deposited asset, can receive claimable collateral, and can earn RIPE when configured.

### How do I earn RIPE rewards?

[RIPE rewards](../earning-and-rewards/03-ripe-rewards.md) accrue from a configured, allowance-capped rate across eligible categories and assets. RIPE is minted when claimed, and a configured claim portion can be deposited into the current core governance vault with a reward lock. See [RIPE Params](https://params.ripe.finance) for current settings.

## GREEN Stability

### How does GREEN maintain its $1 peg?

Six mechanisms work together:

1. **Overcollateralization**: Borrowing is limited by the configured terms of deposited collateral
2. **Dynamic rates**: Borrowing costs can increase when sustained, corroborated reference-pool observations cross the configured danger threshold
3. **Direct redemption**: Eligible GREEN can be exchanged for oracle-valued collateral when a position and all redemption checks qualify
4. **Stability pool redemption**: A separately configured path through available claim collateral
5. [**Endaoment**](../core-protocol/07-endaoment.md) **operations**: Authorized, bounded treasury actions can adjust configured liquidity positions
6. **Peg Stability Module (PSM)**: Optional reserve-backed conversions with independent mint and redemption controls

### What happens if GREEN trades below $1?

Traders may buy discounted GREEN and use an available redemption or PSM path when the resulting value exceeds fees and execution costs. Profitable arbitrage can create buying pressure toward the target, but every path depends on its eligibility, configuration, and available liquidity.

### Can GREEN lose its peg permanently?

Yes. GREEN targets $1, but no mechanism guarantees its market price or recovery time. Overcollateralization, rates, redemptions, Stability pools, Endaoment operations, and the optional PSM provide corrective incentives whose effectiveness depends on collateral, pricing, liquidity, reserves, configuration, and market activity.

### What is the PSM and how do I use it?

When enabled, the [Peg Stability Module](../core-protocol/01-green-stablecoin.md#6-peg-stability-module-psm) converts between GREEN and its configured six-decimal reserve token around a $1 reference. Mint and redeem directions have separate controls, fees, capacity, and liquidity requirements; GREEN can also be delivered through sGREEN when the amount qualifies. See [RIPE Params](https://params.ripe.finance) for current availability and terms.

## Technical Details

### Why did my transaction fail?

When the last-touch guard is enabled, a higher-risk routed action can fail if that user was already touched in the same action block. Wait for the next action block and retry. The action-block source is deployment configuration and can differ from the EVM block clock used for economic timing.

### What is account locking?

Account locking blocks Teller-routed deposits, withdrawals, borrows, and repayments for that account. It is not a universal wallet freeze: direct functions outside Teller, including the PSM and Reserve Engine, follow their own controls.

### Can the protocol be paused?

Ripe has component-level pause and availability controls, not one switch that halts everything. Teller actions, acquisitions, claims, price-source updates, and minting can have different boundaries, so identify the affected component and direction rather than assuming a protocol-wide pause.

## Advanced Features

### Can I use yield-bearing tokens as collateral?

Yes! Tokens like stETH, aTokens, and LP positions continue earning their underlying yields while serving as collateral. The protocol uses share-based accounting to capture all rewards, rebases, and fee accruals.

### Can I use tokenized real-world assets?

Yes! Ripe is built for the $16 trillion in real-world assets being tokenized by 2030. Tokenized stocks, real estate, treasury bills, gold — if it has value, you can borrow against it. This is the core vision: unlocking liquidity for assets that traditional DeFi ignores.

### What's the delegation system?

You can grant granular permissions to other addresses for automated position management:

**Per-Address Delegations:**
* **canWithdraw**: Allow delegate to withdraw collateral on your behalf
* **canBorrow**: Enable borrowing and [deleveraging](../core-protocol/05-deleverage.md) operations
* **canClaimFromStabPool**: Allow claiming liquidated collateral from stability pools
* **canClaimLoot**: Allow claiming RIPE rewards on your behalf

**Global User Settings:**
* **canAnyoneDeposit**: Let any address add collateral for you
* **canAnyoneRepayDebt**: Allow anyone to repay your debt (useful for rescue scenarios)
* **canAnyoneBondForUser**: Let others purchase bonds on your behalf

**Limits and controls:**
* Routed withdrawal proceeds go to the original owner, but a delegate with withdrawal or borrowing authority can still change exposure, debt health, and liquidation risk and can cause material loss
* Each permission is independent — granting one doesn't grant others
* Per-address delegation has no `canDeposit` flag; third-party deposits use the account-wide `canAnyoneDeposit` setting or a trusted protocol path
* Changing or revoking these settings requires the Teller configuration route to be available and unpaused

**Use Cases:**
* Automated yield strategies via third-party protocols
* Team-managed treasury positions
* Bot-assisted position management
* Emergency rescue by trusted parties

### Can I use my borrowed GREEN to earn yield?

Absolutely! When borrowing, you can:

* Receive GREEN directly for any use
* Choose Savings GREEN to convert the borrowed amount into sGREEN
* When configured, send that newly created sGREEN directly to an eligible Stability vault

This can create positive carry when the realized sGREEN return exceeds the GREEN borrowing cost, but neither side of that spread is guaranteed.

## Governance & RIPE Token

### What is RIPE?

RIPE is the protocol's [governance](../governance-and-economics/02-governance.md) token. Lock it in the governance vault to:

* Accumulate governance points that a compatible governance interface can use as voting weight
* Earn rewards from the staker allocation when the asset and category are configured
* Increase the point rate when the configured lock terms provide a duration boost

### How does governance participation work?

Governance-vault positions accumulate points from position size, elapsed blocks, and configured lock terms. A compatible governance interface can use those points as voting weight; the points do not themselves authorize protocol administration. See [Governance](../governance-and-economics/02-governance.md).

### What's the RIPE token distribution?

RIPE has a protocol-wide maximum supply of 1 billion tokens across all blockchains, not a separate 1 billion-token cap per network. The cap is administered across authorized issuance and cross-chain operations; no individual token deployment independently maintains the cross-chain aggregate. The supply allocation is divided as follows:

* **25%** Community incentives (only allocation unlocking at TGE)
* **22.2%** Ripe Foundation treasury
* **20.6%** Core contributors (4-year linear vesting)
* **17.2%** Early backers ($550k seed at $0.02)
* **15%** Distribution partner (Hightop)

Every investor and employee in the Early Backer and Core Contributor allocations agreed to the additional one-year lock. The earliest scheduled investor release or employee position-transfer eligibility under those commitments is June 26, 2027, two years after the June 26, 2025 TGE. A transferred contributor position remains subject to the separate governance-vault lock applied at confirmation. Contributor schedules and lifecycle actions are recorded by onchain vesting contracts, while applicable holder agreements govern the voluntary extension. See full details in [RIPE Tokenomics](../governance-and-economics/01-ripe-tokenomics.md).

### What are Ripe Bonds?

[Bonds](../governance-and-economics/03-bonds.md) let you exchange a configured payment asset for RIPE at an epoch price. A qualifying lock can add a configured bonus, and the accepted payment goes to the [Endaoment](../core-protocol/07-endaoment.md).

### What is the RIPE Reserve Engine?

The [RIPE Reserve Engine](../governance-and-economics/04-reserve-engine.md) accepts a configured payment token and creates a RIPE vesting position. Payment goes to EndaomentFunds at acquisition; RIPE is minted only as vested amounts are claimed, directly or into the current core governance vault.

### What are Bond Boosters?

Bond Boosters can add a configured bonus for eligible bond buyers, subject to their available units, lock requirement, and expiry. See [Ripe Bonds](../governance-and-economics/03-bonds.md).

### What's the Endaoment?

The [Endaoment](../core-protocol/07-endaoment.md) is Ripe's treasury system. Authorized, configured transactions can:

* Manages protocol-owned liquidity from bond sales and Reserve Engine acquisitions
* Deploy treasury assets through [Underscore](https://underscore.finance/) integrations
* Support GREEN's peg through bounded market operations
* Manage protocol-owned liquidity and treasury positions

Accepted bond and Reserve Engine payments become treasury assets held by EndaomentFunds. The Reserve Engine's RIPE allocation is separately minted only as vested claims are paid.

## Safety & Security

### What are the main risks?

* **Smart contract risk**: Bugs could affect funds
* **Liquidation risk**: Collateral value dropping too fast
* **Oracle risk**: Incorrect or unavailable price feeds; ordered fallback improves availability but is not cross-source consensus
* **Interest rate risk**: Dynamic rates during market stress

### Is Ripe audited?

ChainSecurity reviewed an earlier Ripe Finance smart-contract architecture, and Anatomist reviewed a later Ripe Protocol revision. Each review covers only the specific code revisions and scope identified in its report; later changes and deployments may not be covered. See [Audits](audits.md) for the published reports and scope details.

### How does Ripe price assets accurately?

Ripe's [Price Desk](../core-protocol/06-price-oracles.md) checks configured priority sources and then other registered sources, using the first usable price. Source failures are isolated so later sources can answer; if none can, strict valuation fails closed and debt-bearing accounts can enter quarantine.

### How does Ripe handle bad debt?

Conservative collateral terms, redemption, deleverage, [liquidation](../core-protocol/04-liquidations.md), and funded [Stability pools](../earning-and-rewards/02-stability-pools.md) reduce bad-debt risk. Qualifying [bond](../governance-and-economics/03-bonds.md) payments can also clear outstanding bad debt up to their oracle value; the associated RIPE still counts toward the protocol-wide 1 billion-token cap.

### What happens in a market crash?

During extreme volatility:

* Redemption and deleverage remain separate paths for reducing eligible debt
* Stability pools provide liquidation liquidity only while compatible and sufficiently funded
* Target-based liquidation can limit collateral loss when conditions permit; severe shortfalls can produce a full-debt target and exhaust all eligible collateral without fully clearing the debt
* Dynamic rates can increase borrowing cost under sustained configured danger conditions
* Ordered oracle sources provide failover; when none is usable, valuation fails closed and debt-bearing accounts can enter quarantine

## Getting Help

### Where can I learn more?

* [**Documentation**](../): Detailed guides in our docs
* [**Discord**](https://discord.gg/hightop): Active community and team support
* [**Twitter**](https://x.com/ripe_dao): Protocol updates and announcements
* [**GitHub**](https://github.com/Ripe-Foundation/ripe-protocol): Open source code and development
* [**Homepage**](https://www.ripe.finance/): Ripe Homepage

### How do I report bugs or issues?

Security issues: Please report privately to ripefinance@proton.me General bugs: Open an issue on GitHub or report in Discord

***

_This FAQ covers common questions. For detailed technical information, see our comprehensive documentation._
